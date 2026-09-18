# Frigate Configuration

WAMF v0.4.0 uses Frigate as its detection source.

Frigate detects the `bird` object and publishes detection events over MQTT. WAMF receives those events and retrieves the associated snapshot from the Frigate HTTP API for species classification.

WAMF v0.4.0 has been validated with:

- Frigate 0.17.2
- Frigate 0.18.0

## Requirements

Before connecting WAMF to Frigate, ensure that:

- Frigate is running and detecting objects.
- MQTT is enabled in Frigate.
- Frigate and WAMF can access the same MQTT broker.
- At least one Frigate camera is tracking the `bird` object.
- Event snapshots are available through the Frigate HTTP API.
- WAMF can access the Frigate HTTP API over the network.

The camera name configured in WAMF must exactly match the camera identifier configured in Frigate.

## Minimum Camera Example

The following example shows the important parts of a Frigate configuration for WAMF:

```yaml
mqtt:
  enabled: true
  host: 192.168.1.10
  port: 1883

cameras:
  bird_camera:
    ffmpeg:
      inputs:
        - path: rtsp://username:password@camera-address/stream
          roles:
            - detect

    detect:
      enabled: true

    objects:
      track:
        - bird

    snapshots:
      enabled: true
```

Replace the MQTT address, camera credentials, stream address and camera name with values appropriate for your environment.

This example is intentionally minimal. Your Frigate installation may require additional configuration for detectors, hardware acceleration, recording, zones or other features.

## Camera Names

The Frigate camera identifier must match the camera configured in WAMF.

For example:

```yaml
cameras:
  bird_camera:
```

must be configured in WAMF as:

```text
bird_camera
```

Camera names are case-sensitive configuration identifiers and should be copied exactly.

## MQTT

WAMF listens for Frigate detection events using the MQTT broker configured for your installation.

A typical Frigate configuration uses:

```yaml
mqtt:
  enabled: true
  host: 192.168.1.10
  port: 1883
```

WAMF must be configured with the address and port of the same MQTT broker.

If your broker requires authentication, configure the appropriate MQTT credentials in both Frigate and WAMF.

The default Frigate MQTT topic is:

```text
frigate
```

If you change the Frigate topic prefix, configure WAMF to use the corresponding topic.

## Frigate HTTP API

WAMF uses the Frigate HTTP API to retrieve media associated with detection events.

The Frigate API address configured in WAMF must therefore be reachable from the WAMF host or container.

For example:

```text
http://192.168.1.10:5000
```

Do not use `127.0.0.1` or `localhost` unless Frigate is genuinely accessible at that address from the WAMF runtime.

This is particularly important when WAMF runs in Docker or when Frigate runs on another host.

## Detectors and Hardware Acceleration

WAMF does not require a particular Frigate detector or acceleration device.

Frigate may use any detector supported by your Frigate installation, such as a Coral Edge TPU, OpenVINO, TensorRT or another supported detector.

WAMF receives the resulting detection event and does not directly interact with the Frigate detector hardware.

Configure detectors and hardware acceleration according to the Frigate documentation and the hardware available on your Frigate host.

## Recording

Frigate recording is not required simply for WAMF to receive a detection event and classify its event snapshot.

You may enable Frigate recording according to your own retention and surveillance requirements.

For example:

```yaml
record:
  enabled: true
```

Frigate recording retention and WAMF archived media are separate storage concerns and can be managed independently.

## Testing the Connection

Before troubleshooting WAMF classification, confirm that:

1. Frigate detects a `bird` on the configured camera.
2. The event is published over MQTT.
3. WAMF is connected to the MQTT broker.
4. WAMF can reach the Frigate HTTP API.
5. The event snapshot can be retrieved from Frigate.

Once these are working, WAMF can process the event for species classification.

Continue to the [Quick Start](../getting-started/quick-start.md) guide to configure WAMF.