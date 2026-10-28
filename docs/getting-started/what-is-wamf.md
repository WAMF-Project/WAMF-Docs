# What is WAMF?

WAMF stands for **Wildlife Activity Monitoring Framework**.

It is an open-source platform designed to receive wildlife detections from camera systems, classify detected species and build a searchable record of wildlife activity.

WAMF is designed for self-hosted environments such as home labs, wildlife monitoring systems and dedicated observation stations.

## WAMF v0.4.2

WAMF v0.4.2 is the current released version.

This maintenance release improves security and first-run setup while retaining
Python 3.11, the CPU TFLite classifier and AMD64 deployment scope. See the
[release notes](../releases/0.4.2.md) and
[upgrade guide](../deployment/updating.md).

In this release, Frigate provides the object detection layer.

The basic processing flow is:

```text
Camera
  ↓
Frigate
  ↓
MQTT detection event
  ↓
WAMF
  ↓
Species classification
  ↓
Observation database and archived media
  ↓
WAMF web interface
```

Frigate detects the `bird` object and publishes the detection event over MQTT.

WAMF receives the event, retrieves the associated media from the Frigate HTTP API and performs species classification.

The resulting observation is stored in the WAMF database together with its associated archived media.

## What WAMF Provides

WAMF v0.4.2 provides:

- Bird detection event processing
- AI-powered species classification
- Observation history
- Snapshot and clip archiving
- Media retention management
- Species metadata
- Administrative tools
- System health information
- REST API access

## What Frigate Provides

Frigate remains responsible for:

- Camera stream ingestion
- Object detection
- Tracking the `bird` object
- Publishing detection events over MQTT
- Providing event media through its HTTP API

WAMF does not control or require a particular Frigate detector.

The Frigate installation may use any detector supported by Frigate and suitable for the host hardware.

## Deployment

Docker is the recommended deployment method for WAMF v0.4.2.

The official container image is published through the GitHub Container Registry.

See [Installation](installation.md) for deployment options or go directly to [Docker Deployment](../deployment/docker.md).

## Future Development

WAMF is under active development.

Future releases are intended to expand the platform beyond the current Frigate-based detection pipeline and provide greater flexibility for wildlife monitoring systems.

Features described as future development are not part of WAMF v0.4.2 unless specifically documented as such.