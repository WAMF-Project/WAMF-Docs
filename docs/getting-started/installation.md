# Installation

WAMF can be deployed in multiple ways depending on your environment and requirements.

This guide provides an overview of the available installation methods.

## Supported Deployment Methods

### Docker

Docker is the recommended deployment method for most users.

Benefits include:

- Simple installation
- Isolated environment
- Easy upgrades
- Consistent deployments
- Suitable for home servers and virtual machines

See the [Docker Deployment](../deployment/docker.md) guide for details.

### Native Installation

Native installation is intended for development, testing and advanced users who want direct control over the runtime environment.

Benefits include:

- Full access to the Python environment
- Easier debugging
- Flexible customisation
- Suitable for development systems

Detailed native installation instructions will be provided in a future release.

## System Requirements

The exact requirements depend on your deployment method and workload.

Minimum requirements:

- Linux operating system
- Python 3.11 or newer
- Network connectivity to supported camera and detection systems
- Sufficient storage for media and observation data

Recommended requirements:

- Multi-core processor
- 8 GB RAM or more
- SSD storage
- Dedicated AI accelerator (optional)

## Before You Begin

Before installing WAMF, ensure you have:

- A supported Linux system
- Access to a camera or detection source
- Administrative access to the host system
- Basic familiarity with Docker or Python environments

## Next Steps

Choose an installation method:

- Docker Deployment
- Native Installation

Once installed, continue to the Quick Start guide to configure your first WAMF instance.