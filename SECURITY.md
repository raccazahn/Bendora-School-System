# Security Policy - Bendora School System

<span style="color:#0056A3">**Bendora Technology takes data security and student privacy seriously.**</span>

## 📅 Supported Versions
| Version | Supported |
|---------|-----------|
| `main` (development) | ⚠️ Testing only — not for production use |
| `v1.x` (stable) | ✅ Active security patches & updates |
| `v0.x` (legacy) | ❌ End of life — upgrade immediately |

## 🚨 Reporting a Vulnerability
We encourage responsible disclosure. If you discover a security issue:
1. **DO NOT** open a public GitHub issue
2. Email `bendoratechnology@gmail.com` with:
   - Clear description & steps to reproduce
   - Potential impact (data exposure, auth bypass, etc.)
   - Your contact information
3. We will acknowledge within 48 hours and provide a remediation timeline
4. You will be credited in our security acknowledgments (optional)

## 🛡️ Security Architecture
This platform is built with defense-in-depth principles:
✅ **Multi-Tenant Isolation**: PostgreSQL Row-Level Security (RLS) enforces strict data boundaries  
✅ **Authentication**: OIDC/OAuth2 compatible, short-lived JWTs, optional MFA, secure session management  
✅ **Encryption**: TLS 1.3 in transit; AES-256 for sensitive data at rest (PII, grades, health records)  
✅ **Access Control**: Role-Based Access Control (RBAC) with principle of least privilege  
✅ **Audit Logging**: Immutable logs for auth events, data modifications, and admin actions  
✅ **Dependency Hygiene**: Automated SCA (Software Composition Analysis) in CI pipeline  

## 🌍 Compliance & Data Privacy
Designed to meet or exceed standards relevant to educational institutions in Liberia and globally:
- 🇱🇷 **Liberia Data Protection Act (2022)**: Consent, minimization, retention controls
- 🇺🇸 **FERPA/COPPA**: Parental consent workflows, restricted minor data access
- 🇪🇺 **GDPR**: Right to erasure, data portability, breach notification protocols
- 📚 **WAEC/MoE Alignment**: Academic data integrity & audit trails

## 🧪 Security Testing (For Contributors)
Before submitting code, ensure:

# Python security scan
pip-audit
bandit -r src/

# Node dependency audit
npm audit --audit-level=moderate

# Secret/leak detection
gitleaks detect --source .
```

## 🔐 Production Hardening Checklist
- [ ] `APP_ENV=production` (disables debug, enables secure headers)
- [ ] HTTPS enforced with HSTS & CSP
- [ ] Database connections use SSL mode `verify-full`
- [ ] Secrets stored in vault/env manager (never in code)
- [ ] Rate limiting & bot protection active
- [ ] Automated daily backups with offsite encryption

## 📬 Contact
- Security Team: `security@bendora.tech`
- PGP Key: Available upon request
- Response SLA: 48 hours for initial acknowledgment, 7 days for critical fixes

<span style="color:#0A2540">*Security is not a feature — it's the foundation. — Bendora Engineering*</span>
