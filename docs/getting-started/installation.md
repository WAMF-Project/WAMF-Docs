# Installation

WAMF can be deployed in multiple ways depending on your environment and requirements.

This guide provides an overview of the available installation methods. Detailed installation instructions are provided in the relevant deployment guides.

## Supported Deployment Methods

### Docker

Docker is the recommended deployment method for most users.

Benefits include:

- Simple installation
- Isolated environment
- Easy upgrades
- Consistent deployments
- Suitable for home servers and virtual machines

See the [Docker Deployment](../deployment/docker.md) guide for detailed instructions.

### Native Installation

Native installation is intended for development, testing and advanced users who want direct control over the runtime environment.

Benefits include:

- Full access to the Python environment
- Easier debugging
- Flexible customisation
- Suitable for development systems

Detailed native installation instructions will be provided in a future release.

## Requirements

### Minimum System Requirements

- Linux operating system
- Python 3.11 or later
- Multi-core processor
- 8 GB RAM recommended
- Sufficient storage for the WAMF database, snapshots and clips

### WAMF v0.4 Dependencies

The current release of WAMF integrates with Frigate for object detection.

Before installing WAMF, ensure you have:

- A configured Frigate instance with at least one camera
- An MQTT broker accessible to both Frigate and WAMF
- Network access from WAMF to the Frigate API
- A supported camera stream configured in Frigate

WAMF receives detection events from Frigate over MQTT and retrieves associated media using the Frigate API.

Support for additional detection systems is planned for a future release.

## Before You Begin

Before installing WAMF, you should have:

- Administrative access to the host system
- Basic familiarity with Docker or Python environments
- A working Frigate installation

## Next Steps

Choose an installation method:

- Docker Deployment
- Native Installation

Once WAMF is installed, continue to the Quick Start guide to configure your first system.