# WAMF Configuration

WAMF is configured using a single YAML configuration file.

By default, the configuration file is located at:

```text
config/config.yml
```
An example config file is included at:
```text
config/config.yml.example
```


The following example shows a complete WAMF configuration with the default settings.

```yaml

frigate:
  frigate_url: http://127.0.0.1:5000
  mqtt_server: your-mqtt-server
  mqtt_auth: false
  # mqtt_username: your-username
  # mqtt_password: your-password
  mqtt_port: 1883  # Optional, default: 1883
  mqtt_use_tls: false  # Optional, default: false. Use TLS for broker connection
  mqtt_tls_insecure: false  # Optional, default: false. Don't verify broker certificate
  # mqtt_tls_ca_certs: /path/to/ca.crt  # Optional. Path to CA certificate file
  main_topic: frigate
  camera:
    - your-camera-name
  object: bird
# MQTT topics published by whosatmyfeeder:
#   whosatmyfeeder/detections              - common name on every detection above threshold
#   whosatmyfeeder/new_species/common_name     \
#   whosatmyfeeder/new_species/scientific_name  | published once per species,
#   whosatmyfeeder/new_species/score            | first detection ever only
#   whosatmyfeeder/new_species/camera           | (retained by broker)
#   whosatmyfeeder/new_species/frigate_event   /
# Frigate sub-label fallback: if WAMF's classifier is below threshold,
# WAMF will use Frigate's built-in bird classification sub_label if present.
# Enable in Frigate: https://docs.frigate.video/configuration/bird_classification
classification:
  model: model.tflite
  threshold: 0.7
bridge:
  enabled: false
  events_url: http://192.168.1.112:5000/api/events
  timeout_seconds: 1
  health_check_interval_seconds: 60
webui:
  port: 7766
  host: 0.0.0.0

live_view:
  url: http:// # Live view url from go2rtc etc

storage:
  database_path: ./data/speciesid.db

media:
  snapshots_path: ./media/wamf/snapshots
  clips_path: ./media/wamf/clips



retention:
  enabled: true
  snapshots_days: 90
  clips_days: 90
  delete_media: false
  orphan_scan_enabled: true
  
  delete_orphaned_media: false
  system_events_days: 90
  system_events_min_rows: 1000
  config_backups_max_files: 10
  species_overrides:
    Erithacus rubecula: # European Robin
      snapshots_days: 730
      clips_days: 180

admin:
  auth_enabled: true
  # Generate a long random value for session_secret and keep it stable across restarts.
  session_secret: "<long random session secret>"
  session_cookie_samesite: Lax
  session_cookie_secure: false
  # Generate with:
  # python -c "from werkzeug.security import generate_password_hash; print(generate_password_hash('change-me'))"
  password_hash: "<generated werkzeug password hash>"

api:
  token_auth_enabled: true
  token_hash: ""

```

The remainder of this page explains each section of the configuration file.

## Frigate

The `frigate` section configures communication between WAMF and Frigate.

```yaml
frigate:
  frigate_url: http://127.0.0.1:5000
  mqtt_server: your-mqtt-server
  mqtt_auth: false
  # mqtt_username: your-username
  # mqtt_password: your-password
  mqtt_port: 1883  # Optional, default: 1883
  mqtt_use_tls: false  # Optional, default: false. Use TLS for broker connection
  mqtt_tls_insecure: false  # Optional, default: false. Don't verify broker certificate
  # mqtt_tls_ca_certs: /path/to/ca.crt  # Optional. Path to CA certificate file
  main_topic: frigate
  camera:
    - your-camera-name
  object: bird
# MQTT topics published by whosatmyfeeder:
#   whosatmyfeeder/detections              - common name on every detection above threshold
#   whosatmyfeeder/new_species/common_name     \
#   whosatmyfeeder/new_species/scientific_name  | published once per species,
#   whosatmyfeeder/new_species/score            | first detection ever only
#   whosatmyfeeder/new_species/camera           | (retained by broker)
#   whosatmyfeeder/new_species/frigate_event   /
# Frigate sub-label fallback: if WAMF's classifier is below threshold,
# WAMF will use Frigate's built-in bird classification sub_label if present.
# Enable in Frigate: https://docs.frigate.video/configuration/bird_classification
```

WAMF receives detection events from Frigate over MQTT and retrieves snapshots and clips using the Frigate HTTP API.

| Setting | Required | Description |
|----------|:--------:|-------------|
| `frigate_url` | Yes | URL of the Frigate server. |
| `mqtt_server` | Yes | MQTT broker hostname or IP address. |
| `mqtt_auth` | No | Enable MQTT username/password authentication. |
| `mqtt_username` | If mqtt_auth is enabled | MQTT username when authentication is enabled. |
| `mqtt_password` | If mqtt_auth is enabled | MQTT password when authentication is enabled. |
| `mqtt_port` | No | MQTT broker port (default `1883`). |
| `mqtt_use_tls` | No | Enable TLS encryption for MQTT connections. |
| `mqtt_tls_insecure` | No | Disable TLS certificate verification (not recommended). |
| `main_topic` | No | Frigate MQTT topic prefix. Normally `frigate`. |
| `camera` | Yes | List of Frigate camera names monitored by WAMF. |
| `object` | Yes | Object type to classify. Normally `bird`. |

## Classification

The `classification` section controls how WAMF classifies detected objects.

```yaml
classification:
  model: model.tflite
  threshold: 0.7
```

| Setting | Required | Description |
|----------|----------|-------------|
| `model` | Yes | Path to the TensorFlow Lite model used for classification. |
| `threshold` | Yes | Minimum confidence score required for a classification to be accepted. |

---

## External Integration

The `bridge` section configures communication with external systems.

When enabled, WAMF can send system events to an external HTTP endpoint. This can be used to integrate WAMF with dashboards, notification systems, monitoring platforms, or other software capable of receiving event data.

```yaml
bridge:
  enabled: false
  events_url: http://192.168.1.112:5000/api/events
  timeout_seconds: 1
  health_check_interval_seconds: 60
```

| Setting | Required | Description |
|----------|----------|-------------|
| `enabled` | No | Enable or disable external event integration. |
| `events_url` | If enabled | HTTP endpoint that will receive WAMF system events. |
| `timeout_seconds` | No | Maximum time to wait for the remote server to respond. |
| `health_check_interval_seconds` | No | Interval, in seconds, between connectivity checks to the remote endpoint. |

---

## Web UI

The `webui` section configures the built-in WAMF web interface.

```yaml
webui:
  port: 7766
  host: 0.0.0.0
```

| Setting | Required | Description |
|----------|----------|-------------|
| `port` | Yes | TCP port used by the WAMF web interface. |
| `host` | Yes | Network interface to bind the web server to. Use `0.0.0.0` to listen on all interfaces. |

---

## Live View

The `live_view` section configures the live camera stream displayed in the WAMF dashboard.

```yaml
live_view:
  url: http://
```

| Setting | Required | Description |
|----------|----------|-------------|
| `url` | No | URL of the live stream, typically provided by go2rtc or another RTSP-to-web streaming service. |

---

## Storage

The `storage` section defines where WAMF stores its database.

```yaml
storage:
  database_path: ./data/speciesid.db
```

| Setting | Required | Description |
|----------|----------|-------------|
| `database_path` | Yes | Path to the SQLite database used by WAMF. |

---

## Media

The `media` section configures where snapshots and clips are stored.

```yaml
media:
  snapshots_path: ./media/wamf/snapshots
  clips_path: ./media/wamf/clips
```

| Setting | Required | Description |
|----------|----------|-------------|
| `snapshots_path` | Yes | Directory used to store captured snapshots. |
| `clips_path` | Yes | Directory used to store video clips. |

---

## Retention

The `retention` section controls automatic cleanup of media, system events and backups.

```yaml
retention:
  enabled: true
  snapshots_days: 90
  clips_days: 90
  delete_media: false
  orphan_scan_enabled: true
  delete_orphaned_media: false
  system_events_days: 90
  system_events_min_rows: 1000
  config_backups_max_files: 10
  species_overrides: 
    Erithacus rubecula: # European Robin
      snapshots_days: 730
      clips_days: 180
```

| Setting | Required | Description |
|----------|----------|-------------|
| `enabled` | No | Enable automatic retention processing. |
| `snapshots_days` | No | Number of days to retain snapshots. |
| `clips_days` | No | Number of days to retain video clips. |
| `delete_media` | No | Permanently delete expired media files. When disabled, retention is reported but files are not removed. |
| `orphan_scan_enabled` | No | Scan for media files that are no longer referenced by the database. |
| `delete_orphaned_media` | No | Delete orphaned media files found during scans. |
| `system_events_days` | No | Number of days to retain system events. |
| `system_events_min_rows` | No | Minimum number of system events to retain regardless of age. |
| `config_backups_max_files` | No | Maximum number of configuration backups to keep. |
| `species_overrides` | No | Species-specific retention overrides. Leave empty unless custom rules are required. |

---

## Administration

The `admin` section configures authentication for the WAMF web interface.

```yaml
admin:
  auth_enabled: true
  session_secret: "<long random session secret>"
  session_cookie_samesite: Lax
  session_cookie_secure: false
  password_hash: "<generated werkzeug password hash>"
```

| Setting | Required | Description |
|----------|----------|-------------|
| `auth_enabled` | No | Enable or disable administrator authentication. |
| `session_secret` | Yes | Secret key used to sign user sessions. This value must remain the same between restarts. |
| `session_cookie_samesite` | No | SameSite policy applied to the session cookie. |
| `session_cookie_secure` | No | Only send the session cookie over HTTPS. Set to `true` when using HTTPS. |
| `password_hash` | Yes | Werkzeug password hash for the administrator account. |

### Generating a Session Secret

Generate a secure random session secret:

```bash
python -c "import secrets; print(secrets.token_hex(32))"
```

Copy the generated value into the `session_secret` setting.

### Generating an Administrator Password Hash

Generate a Werkzeug password hash:

```bash
python -c "from werkzeug.security import generate_password_hash; print(generate_password_hash('change-me'))"
```

Replace `change-me` with your chosen administrator password.

Copy the generated hash into the `password_hash` setting.

---

## API

The `api` section controls authentication for the REST API.

```yaml
api:
  token_auth_enabled: true
  token_hash: ""
```

| Setting | Required | Description |
|----------|----------|-------------|
| `token_auth_enabled` | No | Enable API token authentication. |
| `token_hash` | If authentication is enabled | SHA-256 hash of the API token. |

### Generating an API Token Hash

Generate a random API token:

```bash
python -c "import secrets; print(secrets.token_hex(32))"
```

Store the generated token somewhere safe, then generate its SHA-256 hash:

```bash
python -c "import hashlib; token='PASTE_TOKEN_HERE'; print(hashlib.sha256(token.encode()).hexdigest())"
```

Copy the resulting hash into the `token_hash` setting.

!!! warning

    The original API token cannot be recovered from its hash. Store it securely before replacing it with the generated hash.