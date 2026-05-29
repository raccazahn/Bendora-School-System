# Contributing to Bendora School System

<span style="color:#0056A3">**Thank you for helping build the future of education technology in Liberia and beyond.**</span>

This guide outlines how engineers, designers, and QA specialists can contribute effectively while maintaining our engineering standards, security practices, and multi-tenant architecture.

## 🎯 Quick Start for Contributors

1. **Fork & Clone**: Fork this repo, clone locally, and create a feature branch:
   ```bash
   git checkout -b feat/your-feature-name
   ```
2. **Setup Environment**: Follow our [Local Dev Guide](docs/LOCAL_DEV_SETUP.md) to run the app locally.
3. **Code**: Implement your feature/fix following our [Engineering Standards](#engineering-standards).
4. **Test**: Ensure all tests pass and coverage meets thresholds.
5. **Document**: Update relevant docs in `/docs/` if behavior changes.
6. **PR**: Submit a Pull Request with a clear description, linked issues, and screenshots (if UI).

## 🔧 Engineering Standards

| Area | Standard |
|------|----------|
| **Backend** | Python 3.11+, FastAPI/Flask, strict type hints, async-first where applicable |
| **Frontend** | React 18+, TypeScript, Tailwind CSS, component-driven architecture |
| **Database** | PostgreSQL 15+, RLS enforced, migrations via Alembic |
| **Code Style** | `black` + `isort` (Python), `eslint` + `prettier` (TS) |
| **Commits** | Conventional Commits: `feat(auth): add MFA toggle` |
| **Multi-Tenancy** | NEVER query without tenant context. Always use `current_tenant_id` filters or RLS policies. |
| **Liberia-Optimized** | Prioritize low-bandwidth UX, offline sync readiness, mobile-first responsive design |

## 🧪 Before You Submit a PR

Run these checks in your terminal:
```bash
# Backend
pytest tests/ -v --cov=src
black src/ tests/
isort src/ tests/

# Frontend
npm run lint
npm run test
npm run format

# Security & Dependencies
pip-audit
npm audit
```

## 📋 Pull Request Checklist
- [ ] Feature matches a tracked issue/PRD requirement
- [ ] All tests pass locally & in CI
- [ ] Code is formatted, linted, and type-safe
- [ ] No hardcoded secrets, tenant IDs, or region-specific overrides
- [ ] Multi-tenant data isolation verified
- [ ] Low-bandwidth/offline UX considered (if frontend)
- [ ] Documentation updated (`/docs/` or inline)
- [ ] Screenshots/GIFs attached (for UI changes)

## 🚫 What We Don't Accept
- PRs that bypass tenant isolation or RLS policies
- Hardcoded credentials or environment-specific URLs
- Unoptimized assets (images > 100KB, heavy JS bundles)
- Breaking changes without version bump + migration guide

## 🤝 Code Review Process
All PRs require at least **1 approval** from a Bendora core engineer. Reviewers will check:
- ✅ Architectural alignment
- ✅ Security & compliance implications
- ✅ Performance & accessibility impact
- ✅ Multi-tenant safety

## 📬 Questions or Blockers?
- 🐛 Bugs: Open an issue with `bug` label
- 💡 Features: Open a Discussion or RFC
- 🛠️ Help: Tag `@bendora/engineering` in your PR comments

<span style="color:#0A2540">*Engineered for impact. Built for scale. Optimized for Liberia.*<br>
— Bendora Technology</span>
```
