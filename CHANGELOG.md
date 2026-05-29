# Changelog — Bendora School System

<span style="color:#0056A3">**All notable changes to this project will be documented in this file.**</span>

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased]
### 🚀 Added
- Initial multi-tenant architecture with PostgreSQL Row-Level Security (RLS)
- Role-based authentication module (Admin, Teacher, Student, Parent)
- Low-bandwidth mode toggle for Liberia connectivity optimization
- Client theming engine with CSS variable overrides
- Demo seeding script for rapid tenant provisioning

### 🔐 Security
- JWT authentication with short-lived tokens + refresh flow
- RLS policies applied to all core tables (`users`, `students`, `courses`, `grades`)
- Environment variable validation via Pydantic settings

### 🇱🇷 Liberia-Optimized
- Default timezone: `Africa/Monrovia`
- WAEC-aligned grading scale validator (1–9)
- SMS fallback trigger hooks for critical alerts
- Asset compression presets for 3G networks

### 🛠️ DevEx
- VS Code launch configurations + recommended extensions
- Docker Compose for local PostgreSQL + Redis
- Alembic migrations with RLS policy hooks

---

## [0.1.0] - 2026-05-30
### 🎉 Initial Release
- ✅ Repository scaffold with engineering-grade documentation suite
- ✅ MIT License + Security Policy + Contribution Guidelines
- ✅ Multi-tenant foundation: `tenant_id` on all models + RLS enforcement
- ✅ Core modules initialized: `auth/`, `academics/`, `attendance/`
- ✅ Client customization workflow: config-driven theming & feature flags
- ✅ Deployment checklist for Render, Docker, and Kubernetes targets

> 🔷 **Note**: This is a foundational release. No public API or UI is stable yet.  
> Intended for internal Bendora Engineering use and pilot client onboarding.

---

## 📋 Versioning Strategy
| Version | Meaning | When to Use |
|---------|---------|-------------|
| `0.x.x` | Pre-release / MVP foundation | Internal dev, pilot clients, architecture validation |
| `1.x.x` | Production-ready for first school launch | Live deployments with SLA |
| `2.x.x` | Major feature expansion (e.g., offline sync, biometrics) | District-scale rollouts |

### 🔄 Release Process
1. Merge features to `main` via PR + CI pass
2. Tag release: `git tag -a v1.2.0 -m "Release v1.2.0"`
3. Update this `CHANGELOG.md` with release notes
4. Push tag: `git push origin v1.2.0`
5. Trigger deployment pipeline (Render/GitHub Actions)

---

## 🗓️ Planned Milestones
| Version | Target | Key Deliverables |
|---------|--------|-----------------|
| `v0.2.0` | Q3 2026 | Gradebook module + WAEC report generator |
| `v0.3.0` | Q4 2026 | Offline sync POC + SMS alert integration |
| `v1.0.0` | Q1 2027 | First production launch (pilot school in Monrovia) |
| `v1.1.0` | Q2 2027 | Parent portal + mobile-responsive PWA |

---

<span style="color:#0A2540">*Versioned with purpose. Documented with rigor. Built for Liberia's future. — Bendora Engineering*</span>
