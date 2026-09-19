# Project Structure

The WAMF repository is organised into several directories and application
modules.

The following structure describes WAMF v0.4.0.

```text
WAMF/
├── app/
├── config/
├── docs/
├── integrations/
├── requirements/
├── routes/
├── scripts/
├── static/
├── templates/
├── tests/
├── retention.py
├── speciesid.py
├── webui.py
├── wamf_paths.py
├── version.py
├── Dockerfile
├── docker-compose.yml
└── requirements.txt
```

## Main Directories

| Path | Description |
|------|-------------|
| `app/` | Core application modules including database access, health monitoring, media archiving, configuration editing, security and species metadata. |
| `config/` | Example configuration files for native and Docker deployments. |
| `integrations/` | Optional WAMF integration modules. |
| `requirements/` | Dependency sets used for the application, testing and development. |
| `routes/` | Web routes for the public interface, administration interface, authentication, API, detections and media. |
| `scripts/` | Supporting scripts used by WAMF and its Docker deployment. |
| `static/` | Stylesheets, images and other static web assets. |
| `templates/` | HTML templates used by the WAMF web interface. |
| `tests/` | Automated test suite. |

## Main Application Files

| Path | Description |
|------|-------------|
| `speciesid.py` | Main wildlife event processing and species identification service. |
| `webui.py` | WAMF web application. |
| `retention.py` | Media retention and cleanup task. |
| `wamf_paths.py` | Shared handling of WAMF application paths. |
| `version.py` | WAMF version information. |

## Deployment Files

| Path | Description |
|------|-------------|
| `Dockerfile` | Builds the WAMF container image. |
| `docker-compose.yml` | Docker Compose definition for WAMF. |
| `requirements.txt` | Top-level Python dependency entry point. |

## Development

Automated tests are located in `tests/`.

Changes to WAMF should keep the existing test suite passing and avoid
introducing regressions into supported native or Docker deployments.