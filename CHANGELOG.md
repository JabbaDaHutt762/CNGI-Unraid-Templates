# Changelog

## 2026-10-10
- Corrected CNGI-WebPortal canonical host mapping to `8190 -> 8080` to match production.


## 2026-10-09

### Added
- Initial public CNGI Unraid template catalog.
- Canonical CNGI-WebPortal DockerMan v2 template.
- Public icon reference that does not depend on access to a private application repository.
- Mandatory application/template synchronization rules.
- Repository handoff documentation.

### WebPortal fresh-install payload
- GHCR image.
- Bridge networking.
- 8190 -> 8080 WebUI port.
- LOGIN_URL.
- SERVICE_REQUEST_URL.
- Project/support/registry metadata.
- Restart policy.

### Data impact
None. This repository contains deployment metadata only.

### Centralized templates and branding
- Added canonical CNGI Core template from its existing production definition.
- Added canonical CNGI Time & Access template from its existing production definition.
- Copied CNGI icon, logo and favicon byte-for-byte from Time & Access into this public catalog.
- Updated Core, Time and WebPortal templates to use the public catalog-hosted CNGI icon.
- Updated all canonical TemplateURL values to this public repository.

### Corrected
- Changed the WebPortal default host port from 8190 to 8190 after verifying Jarvis port assignments. Container port remains 8080; host 8080 is already assigned to Open WebUI.
