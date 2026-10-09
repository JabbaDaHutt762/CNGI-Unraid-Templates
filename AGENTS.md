# CNGI Unraid Templates — Engineering Rules

## Purpose
This public repository is the canonical Unraid deployment catalog for CNGI applications.

## Mandatory synchronization
When a CNGI application release adds, removes, renames or changes a required Docker port, environment variable, volume/path, device, capability, WebUI route or deployment prerequisite, its template in this repository must be updated in the same release work.

Application repositories remain authoritative for application code. This repository is authoritative for fresh Unraid installation metadata.

## Public-repository safety
- Never commit secrets or real credentials.
- Never include private API keys, passwords, access tokens or database credentials as defaults.
- Environment-specific secrets must be entered by the administrator after installation.
- Public URLs used for icons/templates must resolve without GitHub authentication.

## Template quality
Each production template should include:
- Name and exact GHCR repository.
- Network mode.
- WebUI.
- Public icon.
- Project, support and registry metadata.
- Restart policy.
- Every required port, variable, path/device and safe default.
- Useful descriptions.
- TemplateURL pointing to this public repository.

Keep templates valid Unraid DockerMan v2 XML.
