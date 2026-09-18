# Quick Start

This guide covers the minimum steps required to connect a new WAMF v0.4.0 installation to Frigate and begin recording bird observations.

Before continuing:

- WAMF should be installed and running.
- Frigate should already be detecting the `bird` object.
- Frigate and WAMF should have access to the same MQTT broker.
- WAMF must be able to access the Frigate HTTP API.

See [Installation](installation.md) if WAMF has not yet been installed.

## 1. Check Frigate

WAMF v0.4.0 receives detection events from Frigate over MQTT and retrieves associated media through the Frigate HTTP API.

Confirm that:

- Frigate is running.
- MQTT is enabled in Frigate.
- The required camera is configured in Frigate.
- The camera is tracking the `bird` object.
- Snapshots are available through the Frigate API.
- The Frigate HTTP API is accessible from the WAMF host.

The camera name configured in WAMF must exactly match the camera name configured in Frigate.

See [Frigate Configuration](../deployment/frigate.md) for more information.

## 2. Open WAMF

Open the WAMF web interface:

```text
http://<wamf-host>:7767
```

For a new installation, WAMF automatically creates the files required for initial configuration.

If valid administrator credentials do not already exist, WAMF generates an initial administrator password during bootstrap.

For a Docker installation, the temporary password can be found in the container logs:

```bash
docker compose logs wamf
```

Sign in using the generated administrator password.

Change the temporary password after logging in.

## 3. Configure Frigate and MQTT

A new WAMF installation starts in setup mode until the required external services have been configured.

Open the WAMF configuration page and enter the settings for your environment.

The important Frigate and MQTT settings are:

- Frigate HTTP API address
- MQTT broker address
- MQTT port
- MQTT authentication settings, if required
- MQTT topic
- Frigate camera name

A typical installation uses:

```text
MQTT port: 1883
MQTT topic: frigate
```

Use the actual values configured for your environment.

The Frigate camera name entered in WAMF must exactly match the identifier used by Frigate.

For example, if the Frigate configuration contains:

```yaml
cameras:
  bird_camera:
```

the WAMF camera name must be:

```text
bird_camera
```

## 4. Save the Configuration

Save the WAMF configuration after entering the required settings.

WAMF will indicate when a restart is required for configuration changes to take effect.

Use the WAMF restart control when prompted.

After restarting, WAMF should leave setup mode and connect to the configured services.

## 5. Check System Health

Open the WAMF administration interface and check the system health information.

Confirm that the required WAMF services are operating normally and that the MQTT connection is available.

If WAMF cannot communicate with Frigate or MQTT, recheck:

- Hostnames or IP addresses
- MQTT port
- MQTT authentication settings
- Frigate HTTP API address
- Network connectivity between the systems

## 6. Test a Detection

Trigger or wait for a bird detection on the Frigate camera configured in WAMF.

For a successful detection, WAMF should:

1. Receive the Frigate event over MQTT.
2. Retrieve the event snapshot from the Frigate API.
3. Classify the bird species.
4. Store the observation in the WAMF database.
5. Archive the associated media.
6. Display the observation in the WAMF dashboard.

The first classification may take slightly longer while the classifier and supporting components initialise.

## 7. Check the Dashboard

Open:

```text
http://<wamf-host>:7767
```

A successfully processed detection should appear in the WAMF interface.

You can then use the dashboard and observation views to review detections and identified species.

## Troubleshooting

If no observations appear, check:

- The Frigate camera name exactly matches the camera configured in WAMF.
- Frigate is detecting the `bird` object.
- WAMF is connected to the MQTT broker.
- WAMF is using the same MQTT topic as Frigate.
- WAMF can access the Frigate HTTP API.
- Frigate can provide the event snapshot.
- WAMF storage directories are writable.
- The classification result meets the configured confidence threshold.

For Docker installations, check the WAMF logs with:

```bash
docker compose logs -f wamf
```

See the full [Configuration](../reference/configuration.md) guide for configuration details.