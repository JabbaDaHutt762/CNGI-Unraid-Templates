# CNGI Unraid Templates — Project Context

Last updated: 2026-10-09

## Current state
This is the public deployment-template repository for the CNGI ecosystem.

### CNGI WebPortal
Canonical template: `templates/cngi-webportal.xml`

Fresh-install defaults:
- Container: CNGI-WebPortal
- Image: ghcr.io/jabbadahutt762/cngi-webportal:latest
- Network: bridge
- Host WebUI port: 8190
- Container HTTP port: 8080
- LOGIN_URL: optional/blank until Authentik routing is configured
- SERVICE_REQUEST_URL: optional/blank until Core intake is configured
- Public icon: time.clemonsnextgen.com static CNGI icon
- No appdata/database path in WebPortal v0.1.0 because the application is intentionally stateless.

### CNGI Core
Existing application. Its current production Unraid configuration must be audited before publishing a canonical template here. Do not infer credentials or deployment settings.

### CNGI Time & Access
Existing application. Its current production Unraid configuration must be audited before publishing a canonical template here. Do not infer credentials or deployment settings.

## Exact next step
Install the WebPortal template into Unraid's DockerMan templates-user directory, then create CNGI-WebPortal from that template and verify the generated container configuration.
