# WAMF-Docs

Public documentation for WAMF (Wildlife Activity Monitoring Framework).

The site covers installation, Docker deployment, configuration, administration,
API usage, security, backups and troubleshooting for stable WAMF v0.4.3.
Historical release pages retain their original version and validation scope.
Kingfisher v0.5.0 remains under development.

- [Documentation home](docs/index.md)
- [Installation](docs/getting-started/installation.md)
- [Docker deployment](docs/deployment/docker.md)
- [Configuration reference](docs/reference/configuration.md)
- [Administration](docs/administration/overview.md)
- [API reference](docs/reference/rest-api.md)
- [Troubleshooting](docs/reference/faq.md)
- [Project structure](docs/developer-guide/project-structure.md)
- [Application repository](https://github.com/WAMF-Project/WAMF)
- [Public documentation](https://docs.wamf.uk)

## Build locally

Install the pinned documentation dependencies from `requirements.txt` in a
Python virtual environment, then run `mkdocs build --strict`.
Use `mkdocs serve` to preview locally.
