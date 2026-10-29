# Installation

WAMF can be deployed using Docker or installed directly on a Linux host.

For WAMF v0.4.3, Docker is the recommended deployment method for most users.

## Supported Deployment Methods

### Docker

Docker provides the simplest and most consistent way to deploy WAMF.

Benefits include:

- Simple installation
- Isolated runtime environment
- Easy upgrades and container recreation
- Persistent configuration, database and media storage
- Suitable for home servers, virtual machines and dedicated hosts
- Compatible with Docker Compose and Portainer

The official WAMF v0.4.3 image is available from the GitHub Container Registry:

```text
ghcr.io/wamf-project/wamf:0.4.3
```

See the [Docker Deployment](../deployment/docker.md) guide for installation instructions.

### Native Installation

Native installation is intended primarily for development, testing and advanced users who want direct control over the WAMF runtime environment.

A native WAMF v0.4.3 installation requires **Python 3.11**.

Native installation provides:

- Direct access to the Python environment
- Easier debugging and development
- Greater control over services and filesystem permissions
- Access to WAMF without a container runtime

Docker is recommended for normal deployments unless you specifically require a native installation.

## System Requirements

WAMF v0.4.3 requires:

- An AMD64 (x86-64) Linux host for the supported container deployment
- CPU capacity appropriate to the workload; WAMF classification runs on the CPU
- Sufficient storage for the WAMF database, snapshots and clips
- Network connectivity to Frigate and MQTT
- Docker for the recommended container deployment

A multi-core host and 8 GB RAM are provisional host-sizing suggestions, not
measured WAMF minimum requirements. Actual needs depend on workload and other
services. A validated minimum CPU/RAM specification has not been established.

For native installations, Python 3.11 is required.

## WAMF v0.4.3 Dependencies

WAMF v0.4.3 uses Frigate as its detection source.

Before installing WAMF, ensure you have:

- A working Frigate installation
- At least one camera configured in Frigate
- The `bird` object being detected on the required camera
- An MQTT broker accessible to both Frigate and WAMF
- MQTT enabled in Frigate
- Network access from WAMF to the Frigate HTTP API

WAMF receives detection events from Frigate over MQTT and retrieves the associated media through the Frigate HTTP API.

Support for additional detection sources is planned for future WAMF releases.

## Frigate Compatibility

The v0.4.0 compatibility baseline was validated with:

- Frigate 0.17.2
- Frigate 0.18.0

WAMF does not require a particular Frigate detector such as Coral, OpenVINO or TensorRT. Detector configuration is handled by Frigate.

See [Frigate Configuration](../deployment/frigate.md) for the WAMF-specific Frigate requirements.

## Before You Begin

You should have:

- Administrative access to the WAMF host
- Basic familiarity with Docker or Linux administration
- A working Frigate installation
- The address of the MQTT broker used by Frigate
- The address of the Frigate HTTP API
- The Frigate camera name you want WAMF to monitor

The camera identifier configured in WAMF must exactly match the camera name configured in Frigate.

## Choose an Installation Method

For most installations:

[Docker Deployment](../deployment/docker.md)

After WAMF is running, continue to:

[Quick Start](quick-start.md)

The Quick Start guide covers the initial WAMF setup and connection to Frigate.