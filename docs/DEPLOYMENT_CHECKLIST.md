# Deployment Checklist — Bendora School System

<span style="color:#0056A3">**Step-by-step guide to deploying secure, scalable, and Liberia-optimized school portals.**</span>

## 🚀 Pre-Deployment (Staging Validation)
Before pushing to production, verify the following:
- [ ] **CI Pipeline Passed**: Tests, linting, security scans, and build steps completed.
- [ ] **Staging Sync**: Code merged to `main` and deployed to staging (`staging.bendora.school`).
- [ ] **Smoke Tests**:
  ```bash
  curl -I https://staging.bendora.school  # 200 OK
  python scripts/smoke_test.py --env staging
  ```
- [ ] **Low-Bandwidth Check**: Simulate 3G connection (Chrome DevTools) — page load < 3s, asset size < 800KB.
- [ ] **UAT Sign-off**: Client admin verified login, dashboard, and key workflows in staging.

## 🔐 Production Deployment Strategies

### Option A: Render (Recommended for Bendora)
1. **Connect Repo**: Link GitHub repo to Render dashboard.
2. **Set Environment**: 
   - Copy `.env.production` secrets (NEVER commit).
   - Set `APP_ENV=production`, `LOW_BANDWIDTH_MODE=true`.
3. **Build & Start Commands**:
   ```bash
   Build: pip install -r requirements.txt && npm run build
   Start: gunicorn main:app -w 4 -k uvicorn.workers.UvicornWorker
   ```
4. **Database**: Provision PostgreSQL on Render (or external). Run migrations: `alembic upgrade head`.
5. **Auto-Deploy**: Enable from `main` branch. Add custom domain + SSL (Render auto-provisions).

### Option B: Docker + VPS (High Control/Low Latency)
1. **Build Image**: `docker build -t bendora-school:prod .`
2. **Push Registry**: `docker push registry.bendora.tech/school:prod`
3. **Deploy**: 
   ```bash
   ssh user@vps "docker pull registry.bendora.tech/school:prod && docker compose -f docker-compose.prod.yml up -d"
   ```
4. **Reverse Proxy**: Nginx/Traefik handles SSL, caching, and gzip compression.

### Option C: Kubernetes (Enterprise/District Scale)
- Use Helm charts in `infra/k8s/` (see `docs/deployment/K8S_GUIDE.md`).

## ✅ Post-Deployment Verification
Run these immediately after deployment:
- [ ] **Health Check**: 
  ```bash
  curl https://portal.client-school.edu/health  # {"status":"ok","version":"v1.2.0"}
  ```
- [ ] **Auth Flow**: Login as admin → verify role-based dashboard → logout.
- [ ] **Tenant Isolation**: Log in as student from School A → verify NO access to School B data.
- [ ] **Performance**: 
  - TTFB < 200ms (backend)
  - Lighthouse Mobile Score > 85
- [ ] **Security Headers**: 
  ```bash
  curl -I https://portal.client-school.edu | grep -i "strict-transport-security\|content-security-policy"
  ```
- [ ] **Offline/SMS Fallback**: Trigger test alert → verify fallback triggers correctly.

## 🔄 Rollback Procedure (If Needed)
1. **Identify Failure**: Check logs (`docker compose logs -f app` or Render logs).
2. **Quick Revert**: 
   ```bash
   # Render: One-click rollback in dashboard
   # Docker: docker service update --image bendora-school:prev school_service
   ```
3. **Database Rollback** (Only if migration caused corruption): 
   - Restore from last backup: `pg_restore -d bendora_school latest_backup.dump`
   - Run `alembic downgrade -1`
4. **Communication**: Notify client + log incident in `docs/security/INCIDENT_LOG.md`.

## 📊 Monitoring & Alerting Setup
- [ ] **Uptime Monitor**: UptimeRobot/BetterUptime (check `/health` every 2 mins).
- [ ] **Error Tracking**: Sentry integrated for frontend + backend.
- [ ] **Logs**: Centralized via Logtail/Datadog. Filter by `tenant_id` for quick isolation.
- [ ] **Alerts**: Slack/Email for 5xx spikes, auth failures > 10/min, DB connection pool exhaustion.
- [ ] **Backups**: Automated daily DB dumps + weekly offsite (S3/Glacier). Verify restore quarterly.

## 🇱🇷 Liberia-Specific Deployment Notes
- ✅ **Data Residency**: Ensure data stays within compliant regions (EU/US or local MoE-approved).
- ✅ **Connectivity**: Enable `ASSET_COMPRESSION` and `SMS_FALLBACK` by default.
- ✅ **Power Fluctuations**: Configure auto-restart policies on VPS (`Restart=always` in systemd).

<span style="color:#0A2540">*Deploy with confidence. Monitor relentlessly. Scale securely. — Bendora Engineering*</span>
