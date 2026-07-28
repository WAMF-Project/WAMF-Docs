# REST API

WAMF provides a REST API for integrating with external applications.

## Authentication

API authentication can be enabled in `config.yml`.

When enabled, requests must include a valid API token.

## Base URL

```
http://<wamf-host>:7766
```

## Response Format

Responses are returned as JSON.

Example:

```json
{
  "status": "ok"
}
```

## Endpoints

The REST API is under active development.

Additional endpoints will be documented as they become available.

## Errors

Standard HTTP status codes are returned.

| Code | Meaning |
|------|---------|
| 200 | Request successful |
| 400 | Invalid request |
| 401 | Authentication failed |
| 404 | Resource not found |
| 500 | Internal server error |