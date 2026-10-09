# CNGI Unraid Templates — Project Context

Last updated: 2026-10-09

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
- 8187 -> 8080
- LOGIN_URL and SERVICE_REQUEST_URL optional/blank
- no appdata/database mapping in v0.1.0

## Exact next step
Install/refresh these XML files in Unraid DockerMan `templates-user`, then create WebPortal from the canonical template and verify its generated container definition.
