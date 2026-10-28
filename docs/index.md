# Wildlife Activity Monitoring Framework (WAMF)

WAMF is an open-source platform for monitoring, identifying and recording wildlife activity using cameras and AI-powered species classification.

## Current Status

!!! info "Current Release"

    The current release is **WAMF v0.4.2**.

    WAMF v0.4.2 provides a stable Frigate-based detection pipeline, species classification, observation storage, archived media and a web-based administration interface.

    Docker is the recommended deployment method for v0.4.2.

Read the [v0.4.2 release notes](releases/0.4.2.md) for the security and setup
changes, [upgrade instructions](deployment/updating.md) for existing
installations, and [security guidance](reference/security.md) for retained risks.

## Features

- Wildlife detection events received from Frigate
- AI-powered bird species classification
- Observation database
- Snapshot and clip archiving
- Media retention management
- Species metadata enrichment
- Administrative dashboard
- System health monitoring
- REST API

## How WAMF Works

WAMF v0.4.2 uses Frigate as its detection source.

When Frigate detects a bird, it publishes the event over MQTT. WAMF receives the event, retrieves the associated media from Frigate, performs species classification and stores the resulting observation.

The observation and archived media can then be viewed through the WAMF web interface.

## Getting Started

New users should begin with:

1. [What is WAMF?](getting-started/what-is-wamf.md)
2. [Installation](getting-started/installation.md)
3. [Quick Start](getting-started/quick-start.md)

For the recommended container deployment, see [Docker Deployment](deployment/docker.md).

## Documentation Sections

- **Getting Started** - installation and first steps
- **User Guide** - using WAMF day-to-day
- **Administration** - configuration, health and maintenance
- **Deployment** - running WAMF in production
- **Developer Guide** - architecture and development
- **Reference** - configuration and API details

## Project Links

- [WAMF on GitHub](https://github.com/WAMF-Project/WAMF)
- [WAMF Documentation](https://docs.wamf.uk)