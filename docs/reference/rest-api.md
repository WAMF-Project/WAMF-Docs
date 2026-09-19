# REST API

WAMF provides a small REST API that can be used by external applications
and dashboards.

WAMF v0.4.0 provides endpoints for accessing recent detection information
and system health.

## Base URL

By default, WAMF is available on port `7767`.

```text
http://<wamf-host>:7767
```

Replace `<wamf-host>` with the hostname or IP address of your WAMF server.

## Response Format

API responses are returned as JSON.

## Recent Detections

```http
GET /api/detections/recent
```

Returns recent detections recorded by WAMF.

The `limit` query parameter can be used to control the number of detections
returned.

For example, to retrieve only the most recent detection:

```http
GET /api/detections/recent?limit=1
```

The response is a JSON array. Detection data includes fields such as:

- `common_name`
- `display_name`
- `confidence_percent`
- `detection_time`

This endpoint can be used by dashboards and other applications that need
access to recent WAMF detection information.

## System Health

```http
GET /admin/api/health
```

Returns the current health of the WAMF installation.

The response includes health and status information such as:

- `archive_storage`
- `database`
- `disk_used_percent`
- `frigate`
- `mqtt`
- `last_retention_run`
- `missing_count`
- `orphan_count`
- `version`

### Authentication

The health endpoint supports WAMF API token authentication.

When token authentication is enabled, include the API token in the
`X-WAMF-API-Key` request header:

```http
X-WAMF-API-Key: <your-api-token>
```

API tokens can be generated or regenerated from **Admin → API Token**.

!!! warning "Keep API tokens private"
    Treat the API token as a password. Do not include a real token in
    documentation, screenshots or configuration examples that will be
    published publicly.

## Example

A health request using `curl` can be made with:

```bash
curl \
  -H "X-WAMF-API-Key: <your-api-token>" \
  http://<wamf-host>:7767/admin/api/health
```

## Errors

WAMF uses standard HTTP status codes when processing API requests.

For example, an authentication failure on a protected endpoint returns an
HTTP error rather than health data.


### Homepage Example

The recent detections endpoint can be used with
[Homepage](https://gethomepage.dev/) and its `customapi` widget to display
the most recent WAMF detection.

```yaml
- WAMF:
    href: http://<wamf-host>:7767/
    description: Wildlife Activity
    widget:
      type: customapi
      url: http://<wamf-host>:7767/api/detections/recent?limit=1
      refreshInterval: 10000
      display: block
      mappings:
        - field: 0.common_name
          label: Species

        - field: 0.display_name
          label: Scientific

        - field: 0.confidence_percent
          label: Confidence
          format: percent

        - field: 0.detection_time
          label: Seen
```

Replace `<wamf-host>` with the hostname or IP address of your WAMF server.

This displays the species, scientific name, confidence and detection time
from the most recent WAMF observation.