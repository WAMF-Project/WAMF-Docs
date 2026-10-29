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

## Configuration file override

`WHOSATMYFEEDER_CONFIG` selects another existing configuration file. A relative
override is resolved from the caller's working directory; a missing override is
an error. This is separate from the application-relative storage paths below.
`WAMF_SECRET_KEY` overrides the configured session secret and must remain stable.

## Complete Example

The following is an illustrative configuration template, not a provisioned
installation. It includes placeholders and an optional species-retention override.
For a new installation, let normal startup create the configuration and use the
private setup link; do not replace an existing installation's credentials.

```yaml
frigate:
  frigate_url: http://127.0.0.1:5000
  mqtt_server: your-mqtt-server
  mqtt_auth: false
  # mqtt_username: your-username
  # mqtt_password: your-password
  mqtt_port: 1883  # Optional, default: 1883
  mqtt_use_tls: false  # Optional, default: false
  mqtt_tls_insecure: false  # Optional, default: false
  # mqtt_tls_ca_certs: /path/to/ca.crt
  main_topic: frigate
  camera:
    - your-camera-name
  object: bird

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
  port: 7767
  host: 0.0.0.0
  # Optional external origin for the first-run setup link.
  # public_base_url: https://wamf.example

live_view:
  url: http://

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
  session_secret: "<long random session secret>"
  session_cookie_samesite: Lax
  session_cookie_secure: false
  password_hash: "<generated werkzeug password hash>"

api:
  token_auth_enabled: true
  token_hash: ""
```

The remainder of this page explains each section of the configuration file.

## Frigate

In Docker, `127.0.0.1` refers to WAMF's container. Use a Frigate address reachable
from that container. MQTT and certificate paths must likewise be reachable or
available inside the runtime.

The `frigate` section configures communication between WAMF and Frigate.

```yaml
frigate:
  frigate_url: http://127.0.0.1:5000
  mqtt_server: your-mqtt-server
  mqtt_auth: false
  # mqtt_username: your-username
  # mqtt_password: your-password
  mqtt_port: 1883
  mqtt_use_tls: false
  mqtt_tls_insecure: false
  # mqtt_tls_ca_certs: /path/to/ca.crt
  main_topic: frigate
  camera:
    - your-camera-name
  object: bird

# Frigate sub-label fallback: if WAMF's classifier is below threshold,
# WAMF will use Frigate's built-in bird classification sub_label if present.
# Enable in Frigate: https://docs.frigate.video/configuration/bird_classification
```

WAMF receives detection events from Frigate over MQTT and retrieves snapshots
and clips using the Frigate HTTP API.

| Setting | Required | Description |
|----------|:--------:|-------------|
| `frigate_url` | Yes | URL of the Frigate server. |
| `mqtt_server` | Yes | MQTT broker hostname or IP address. |
| `mqtt_auth` | No | Enable MQTT username/password authentication. |
| `mqtt_username` | If `mqtt_auth` is enabled | MQTT username when authentication is enabled. |
| `mqtt_password` | If `mqtt_auth` is enabled | MQTT password when authentication is enabled. |
| `mqtt_port` | No | MQTT broker port. Default: `1883`. |
| `mqtt_use_tls` | No | Enable TLS encryption for MQTT connections. |
| `mqtt_tls_insecure` | No | Disable TLS certificate verification. Not recommended. |
| `mqtt_tls_ca_certs` | No | Path to a CA certificate file used to verify the MQTT broker when using TLS. |
| `main_topic` | No | Frigate MQTT topic prefix. Normally `frigate`. |
| `camera` | Yes | List of Frigate camera names monitored by WAMF. |
| `object` | Yes | Object type to classify. Normally `bird`. |

!!! note "Camera names"
    Camera names must match the corresponding Frigate camera names exactly.

## Classification

The `classification` section controls how WAMF classifies detected objects.

```yaml
classification:
  model: model.tflite
  threshold: 0.7
```

| Setting | Required | Description |
|----------|:--------:|-------------|
| `model` | Yes | Path to the TensorFlow Lite model used for classification. |
| `threshold` | Yes | Minimum confidence score required for a classification to be accepted. |

---

## External Event Integration

The `bridge` section configures communication with external systems.

When enabled, WAMF can send system events to an external HTTP endpoint. This
can be used to integrate WAMF with dashboards, notification systems,
monitoring platforms, or other software capable of receiving event data.

```yaml
bridge:
  enabled: false
  events_url: http://<external-host>:5000/api/events
  timeout_seconds: 1
  health_check_interval_seconds: 60
```

| Setting | Required | Description |
|----------|:--------:|-------------|
| `enabled` | No | Enable or disable external event integration. |
| `events_url` | If enabled | HTTP endpoint that will receive WAMF system events. |
| `timeout_seconds` | No | Maximum time to wait for the remote server to respond. |
| `health_check_interval_seconds` | No | Interval, in seconds, for WAMF system health checks and overall health-state transition reporting; not a Bridge endpoint polling interval. |

---

## Web UI

The `webui` section configures the built-in WAMF web interface.

```yaml
webui:
  port: 7767
  host: 0.0.0.0
  # Optional external origin for the first-run setup link.
  # public_base_url: https://wamf.example
```

| Setting | Required | Description |
|----------|:--------:|-------------|
| `port` | Yes | TCP port used by the WAMF web interface. |
| `host` | Yes | Network interface to bind the web server to. Use `0.0.0.0` to listen on all interfaces. |

---

### Public setup-link origin

`webui.public_base_url` is optional. It controls the origin printed in the
first-run administrator setup link, for example `https://wamf.example` or
`http://localhost:17767`. It must use HTTP or HTTPS and contain no credentials,
path prefix, query or fragment. Subpath proxy deployments are not supported by
this setting.

Without it, WAMF prints `127.0.0.1` and the configured web port. For access from
another machine, use the reachable host and published port while retaining the
setup path and token fragment. Use HTTPS or a trusted tunnel for remote setup.

This setting does not change the listening address, enable TLS, configure
trusted-proxy parsing or restrict network access. WAMF does not derive it from
request headers. See [Authentication](../administration/authentication.md).

## Live View

`camera.live_view_url` takes precedence when present; `live_view.url` is the
fallback. Avoid defining both with different values. An empty primary value also
overrides the fallback.

The `live_view` section configures the live camera stream displayed in the
WAMF dashboard.

```yaml
live_view:
  url: http://
```

| Setting | Required | Description |
|----------|:--------:|-------------|
| `url` | No | URL of the live stream, typically provided by go2rtc or another RTSP-to-web streaming service. |

---

## Storage

Relative database and media paths resolve beneath the WAMF application directory,
which is `/app` in the container. They do not follow an arbitrary shell working
directory. Custom paths must be included in your persistent mounts and backups.

The `storage` section defines where WAMF stores its database.

```yaml
storage:
  database_path: ./data/speciesid.db
```

| Setting | Required | Description |
|----------|:--------:|-------------|
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
|----------|:--------:|-------------|
| `snapshots_path` | Yes | Directory used to store captured snapshots. |
| `clips_path` | Yes | Directory used to store video clips. |

---

## Retention

The `retention` section controls how WAMF handles media, system events and
configuration backups when the retention task runs.

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
|----------|:--------:|-------------|
| `enabled` | No | Enable retention processing when the retention task is run. |
| `snapshots_days` | No | Number of days to retain snapshots. |
| `clips_days` | No | Number of days to retain video clips. |
| `delete_media` | No | Permanently delete expired media files. When disabled, retention is reported but files are not removed. |
| `orphan_scan_enabled` | No | Scan for media files that are no longer referenced by the database. |
| `delete_orphaned_media` | No | Delete orphaned media files found during scans. |
| `system_events_days` | No | Number of days to retain system events. |
| `system_events_min_rows` | No | Minimum number of system events to retain regardless of age. |
| `config_backups_max_files` | No | Maximum number of configuration backups to keep. |
| `species_overrides` | No | Species-specific retention overrides. Leave empty unless custom rules are required. |

The retention task must be run separately, for example using cron. The
`enabled` setting controls whether retention processing occurs when the task
is executed; it does not schedule the task itself.

---

## Administration

The `admin` section configures authentication for the WAMF administration
interface.

```yaml
admin:
  auth_enabled: true
  session_secret: "<long random session secret>"
  session_cookie_samesite: Lax
  session_cookie_secure: false
  password_hash: "<generated werkzeug password hash>"
```

| Setting | Required | Description |
|----------|:--------:|-------------|
| `auth_enabled` | No | Enable or disable administrator authentication. |
| `session_secret` | After provisioning | Session-signing secret, generated by fresh bootstrap unless supplied by `WAMF_SECRET_KEY`. Keep it stable across restarts. |
| `session_cookie_samesite` | No | SameSite policy applied to the session cookie. |
| `session_cookie_secure` | No | Only send the session cookie over HTTPS. Set to `true` when using HTTPS. |
| `password_hash` | After setup | Stored administrator credential hash, created through the first-run setup flow. |

On a fresh authentication-enabled installation, startup provides a private,
single-use setup link. The administrator chooses a password within 15 minutes;
WAMF stores its hash. Existing valid passwords, API credentials and session
secrets are preserved. Malformed or unsupported authentication configuration
fails closed instead of resetting credentials. See
[Authentication](../administration/authentication.md) for setup and log protection.

The example values above are placeholders, not usable credentials. Use the
normal bootstrap/setup flow for a new installation rather than copying example
secrets over a provisioned configuration. Keep the session secret stable across
restarts and upgrades, including any `WAMF_SECRET_KEY` environment override.
Use `session_cookie_secure: true` when serving through HTTPS.

The administrator password can subsequently be changed from
**Admin → Password**.

---

## API

The `api` section controls authentication for protected WAMF API endpoints.

```yaml
api:
  token_auth_enabled: true
  token_hash: ""
```

| Setting | Required | Description |
|----------|:--------:|-------------|
| `token_auth_enabled` | No | Enable API token authentication. |
| `token_hash` | If authentication is enabled | Stored credential hash generated through Admin → API Token, using Werkzeug. A bare SHA-256 digest is not a valid replacement. |

API tokens should normally be generated and managed through
**Admin → API Token**.

WAMF stores only the hash of the API token. The original token cannot be
recovered later.

Generating a new token invalidates the previous token. Applications using
the WAMF API must be updated with the new token.

For information about using the available API endpoints, see
[REST API](rest-api.md).