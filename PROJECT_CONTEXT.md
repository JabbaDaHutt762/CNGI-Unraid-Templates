## 2026-10-18 — Staged Core + Time Docker consolidation (draft PR #1)
- Companion to Core draft PR #36 (feature/consolidate-time-into-core-20261018): owner wants Time employee/HR, clock, payroll and billing integrated in Core, retaining the SAME PostgreSQL cngi_core tables/rows. Time Docker stays running until owner validates.
- Core template now stages opt-in CNGI_EMBEDDED_TIME_ENABLED (default false), optional TIME_DATABASE_URL for existing Time least-privilege DB credentials, CALENDAR_SECRET_KEY carrying forward Time's encryption secret, employee Authentik API token/issuer, Google OAuth callback override, and Core time-worker toggle (default false).
- IMPORTANT: Never enable CNGI_CORE_TIME_WORKERS_ENABLED while old Time container still runs. Requires controlled worker handover after Time stops, to avoid duplicate Calendar processing or Authentik changes.
- Added optional remote Tower /backups mount to Core. This **does not** itself implement Time's old automatic image-update database backup/migration runner. Verify Tower backup/restore separately before any update.
- No secrets in repo, no data changes, no user Unraid template replaced; changes live on review branch only.
- Current next step: review/validate Core PR, HR/time permission model and CI, verify backups, then deploy opt-in for trial. Do not delete existing Time template yet.

# CNGI Unraid Templates — Project Context

Last updated: 2026-10-18

## Current state
This public repository is the canonical fresh-install deployment catalog for all three current CNGI applications.

### Canonical templates
- `templates/cngi-core.xml` — copied from the existing Core production template; public TemplateURL/icon centralized here.
- `templates/cngi-time-access.xml` — copied from the existing Time & Access production template; public TemplateURL/icon centralized here.
- `templates/cngi-webportal.xml` — WebPortal v0.1.0 fresh-install template.

### Public branding
The canonical icon and supporting branding files were copied byte-for-byte from the Time & Access repository:
- `icons/cngi-icon-v2.webp`
- `icons/cngi-logo-v2.webp`
- `icons/cngi-favicon.png`

All templates use the public raw URL for `icons/cngi-icon-v2.webp`.

### Important behavior
This catalog supplies Unraid's initial container form. It does not embed secrets. Existing production containers keep their current environment values when updated; a fresh Core or Time install still requires administrator-specific database/Auth credentials.

### WebPortal defaults
- CNGI-WebPortal
- ghcr.io/jabbadahutt762/cngi-webportal:latest
- bridge
- 8190 -> 8080
- LOGIN_URL and SERVICE_REQUEST_URL optional/blank
- no appdata/database mapping in v0.1.0

## Exact next step
Install/refresh these XML files in Unraid DockerMan `templates-user`, then create WebPortal from the canonical template and verify its generated container definition.
