# Quick Start

This guide walks through the minimum steps required to connect WAMF to an existing Frigate installation and begin recording wildlife observations.

Before continuing, make sure WAMF has been installed and Frigate is already detecting birds.

See [Installation](installation.md) for deployment requirements and installation options.

## 1. Check Frigate

WAMF currently receives detection events from Frigate over MQTT and retrieves media through the Frigate HTTP API.

Confirm that:

- Frigate is running.
- MQTT is enabled in Frigate.
- The required camera is configured in Frigate.
- The camera is tracking the `bird` object.
- Snapshots and recordings are enabled.
- The Frigate HTTP API is accessible from the WAMF host.

The camera name used in WAMF must exactly match the camera name configured in Frigate.

## 2. Create the WAMF Configuration

Copy the example configuration file into the WAMF configuration directory if this was not already done during installation.

Open `config/config.yml` and configure the Frigate connection:

```yaml
frigate:
  frigate_url: http://192.168.1.100:5000
  mqtt_server: 192.168.1.100
  mqtt_port: 1883
  mqtt_auth: false
  mqtt_use_tls: false
  main_topic: frigate
  camera:
    - bird_camera
  object:
    - bird
```

Replace the example addresses and camera name with values from your own Frigate installation.

For MQTT brokers that require authentication:

```yaml
frigate:
  mqtt_auth: true
  mqtt_username: "<mqtt username>"
  mqtt_password: "<mqtt password>"
```

## 3. Configure Classification

Set the classification model and minimum confidence threshold:

```yaml
classification:
  model: model.tflite
  threshold: 0.7
```

The threshold is expressed as a value between `0` and `1`.

A threshold of `0.7` requires at least 70% classification confidence before WAMF accepts the result.

## 4. Configure Storage

Set the database and media locations:

```yaml
storage:
  database_path: ./data/speciesid.db

media:
  snapshots_path: ./media/wamf/snapshots
  clips_path: ./media/wamf/clips
```

Make sure the WAMF process has permission to create and modify files in these directories.

## 5. Configure Administrator Access

Generate a session secret:

```bash
python -c "import secrets; print(secrets.token_hex(32))"
```

Generate an administrator password hash:

```bash
python -c "from werkzeug.security import generate_password_hash; print(generate_password_hash('change-me'))"
```

Replace `change-me` with the password you want to use.

Add the generated values to the configuration:

```yaml
admin:
  auth_enabled: true
  session_secret: "<generated session secret>"
  session_cookie_samesite: Lax
  session_cookie_secure: false
  password_hash: "<generated password hash>"
```

Keep the session secret stable across restarts. Changing it will invalidate existing login sessions.

## 6. Start WAMF

Start WAMF using the deployment method selected during installation.

For a native installation:

```bash
python run.py
```

For Docker:

```bash
docker compose up -d
```

Check the application logs if WAMF does not start successfully.

## 7. Open the Dashboard

By default, the WAMF web interface listens on port `7766`.

Open:

```text
http://<wamf-host>:7766
```

Sign in using the administrator password used when generating the password hash.

## 8. Test a Detection

Trigger or wait for a bird detection on the configured Frigate camera.

WAMF should:

1. Receive the Frigate event over MQTT.
2. Retrieve the associated media from Frigate.
3. Classify the detected bird.
4. Store the observation in its database.
5. Display the observation in the dashboard.

The first classification may take slightly longer while the model and supporting components initialise.

## Troubleshooting

If no observations appear, check:

- The Frigate camera name matches the name in `config.yml`.
- Frigate is tracking the `bird` object.
- WAMF can connect to the MQTT broker.
- WAMF can access the Frigate HTTP API.
- The configured media and database directories are writable.
- The classification confidence exceeds the configured threshold.

See the full [Configuration](../deployment/configuration.md) guide for every available setting.