# Client Customization Guide — Bendora School System

<span style="color:#0056A3">**How to white-label, configure, and deploy this portal for any school client — without forking the codebase.**</span>

> 🔷 **Core Philosophy**: `Config > Code`. All customizations are handled via tenant-specific configuration files, environment overrides, and feature flags. This ensures seamless updates and zero maintenance drift.

## 🎨 1. Theming & Branding
Each client inherits the default "Engineering Blue" theme. Override per tenant using JSON config:

// config/clients/{tenant_slug}/theme.json
{
  "brand": {
    "name": "St. Theresa High School",
    "logo_light": "/assets/clients/st-theresa/logo-light.svg",
    "logo_dark": "/assets/clients/st-theresa/logo-dark.svg",
    "favicon": "/assets/clients/st-theresa/favicon.ico",
    "colors": {
      "primary": "#1B5E20",      // Overrides #0056A3
      "secondary": "#0A2540",    // Overrides #0A2540
      "accent": "#1E88E5",
      "background": "#FAFAFA",
      "text_primary": "#212121"
    },
    "typography": {
      "heading": "Inter, system-ui, sans-serif",
      "body": "Roboto, system-ui, sans-serif",
      "scale_ratio": 1.2
    }
  }
}


### 💡 CSS Variable Override (Advanced)
For pixel-perfect brand matching, inject tenant-specific CSS at build/runtime:

/* config/clients/{tenant_slug}/overrides.css */
:root[data-tenant="st-theresa"] {
  --bendora-primary: #1B5E20;
  --bendora-header-gradient: linear-gradient(135deg, #0A2540 0%, #1B5E20 100%);
  --bendora-card-radius: 12px;
  --bendora-font-scale: 1.1;
}

## ⚙️ 2. Feature Toggles & Module Control
Enable/disable academic and administrative modules per school:

# config/clients/{tenant_slug}/features.yaml
modules:
  dashboard: true
  gradebook: true
  attendance: true
  assignment_submission: true
  messaging: false          # Disable if client uses WhatsApp/external SMS
  parent_portal: true
  calendar_sync: true
  offline_mode: true        # Critical for low-connectivity areas

integrations:
  sis_sync:
    enabled: false
    provider: custom_api
  payment_gateway:
    enabled: true
    provider: paystack_mt_liberia  # or custom
  sms_alerts:
    enabled: true
    provider: twilio_africastalking

## 🗄️ 3. Tenant Provisioning Workflow
Spin up a new school instance in under 5 minutes:


# 1. Generate tenant schema & RLS policies
python cli/provision.py --slug st-theresa --admin-email admin@sttheresa.edu.lr

# 2. Import initial data (optional CSV/JSON)
python cli/import_data.py --slug st-theresa --file students_2026.csv --type enrollment

# 3. Link config files
ln -sf config/clients/st-theresa/* config/active/

# 4. Validate & restart
python cli/validate_config.py --slug st-theresa
docker compose restart app-worker

## 🌍 4. Localization & Liberia-Specific Configuration
Optimized for Liberian educational standards and infrastructure:

# config/clients/{tenant_slug}/localization.yaml
region:
  timezone: Africa/Monrovia
  currency: LRD
  academic_year: "2025-2026"
  grading_system: waec_1_9_scale  # or moe_standard, ib, us_gpa
  attendance_policy:
    min_required_pct: 75
    term_structure: 3_terms
  data_retention_days: 730       # Aligns with MoE/WAEC audit requirements
connectivity:
  offline_sync: true
  asset_compression: high        # Reduces page weight by ~60%
  sms_fallback: true             # Critical alerts via SMS when data fails

## 📦 5. Client Delivery Checklist
Before handing over to the school administration:

- [ ] Tenant provisioned with strict RLS isolation
- [ ] Theme & branding assets uploaded & verified
- [ ] Feature flags configured per contract scope
- [ ] Admin superuser created + credentials delivered via secure channel
- [ ] Test enrollment flow completed (student → teacher → grade → report)
- [ ] Backup schedule configured (daily DB + weekly offsite)
- [ ] Low-bandwidth mode tested (3G simulation < 3s load time)
- [ ] UAT sign-off received from client representative

## 🛡️ 6. Security & Compliance Notes
- ✅ All tenant configs are sanitized before loading (no code injection)
- ✅ PII fields masked in logs & UI by default
- ✅ Exportable audit trails for MoE/WAEC compliance reviews
- ✅ Data residency enforced per `config/clients/{tenant_slug}/data_policy.json`

<span style="color:#0A2540">*Customizable by design. Secure by default. Built for Liberian schools. — Bendora Technology*</span>
