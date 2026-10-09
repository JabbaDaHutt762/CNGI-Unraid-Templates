# CNGI Unraid Templates

Public Unraid deployment catalog for the Clemons NextGen Integrations application ecosystem.

## Applications

| Application | Docker image | Default WebUI port | Status |
|---|---|---:|---|
| CNGI Core | `ghcr.io/jabbadahutt762/jarvis-cngi-core:latest` | 8189 | Ready |
| CNGI Time & Access | `ghcr.io/jabbadahutt762/jarvis-cngi-time-access:latest` | 8188 | Ready |
| CNGI WebPortal | `ghcr.io/jabbadahutt762/cngi-webportal:latest` | 8190 | Ready |

This repository contains **deployment metadata only**. It contains no application source code and no secrets.

## Unraid

Templates under `templates/` define the fields Unraid needs when a container is first created: image, ports, environment variables, paths, WebUI, icon, descriptions and defaults.

The application Docker image cannot populate Unraid's Add Container form by itself. A template must first be present in Unraid's Docker template system.

## Security

Never commit passwords, tokens, API keys, database passwords, private hostnames, customer information, or environment-specific secrets here. Defaults must be safe for a public repository.

## Public branding assets
- `icons/cngi-icon-v2.webp` — canonical Unraid container icon used by all three templates.
- `icons/cngi-logo-v2.webp` and `icons/cngi-favicon.png` — shared public branding assets available for deployment tooling.
