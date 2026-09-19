# Frequently Asked Questions

## Does WAMF replace Frigate?

No.

In WAMF v0.4.0, Frigate provides object detection and detection events.

WAMF receives those events and adds wildlife-specific classification,
observation storage, media archiving, analytics and management.

---

## Does WAMF perform object detection?

Not in the current release.

WAMF v0.4.0 uses detection events provided by Frigate. WAMF then performs
additional wildlife classification on those detections.

---

## Does WAMF require an Internet connection?

WAMF's core detection processing and local web interface can operate on your
local network without an Internet connection.

Some optional functionality, such as retrieving species metadata, requires
Internet access.

---

## Can WAMF monitor multiple cameras?

Yes.

Multiple Frigate cameras can be configured in `config.yml`.

Camera names configured in WAMF must match the corresponding Frigate camera
names exactly.

See the [Configuration Reference](configuration.md) for details.

---

## Can WAMF identify animals other than birds?

WAMF v0.4.0 is primarily designed for bird monitoring.

Support for additional wildlife is planned for future development.

---

## Where are observations stored?

Observations are stored in the WAMF SQLite database.

By default, the database is stored in:

```text
data/speciesid.db
```

Associated snapshots and clips are stored under the `media` directory.

These locations can be changed in the WAMF configuration.

---

## Can I back up my observations?

Yes.

A WAMF backup should include the persistent:

- `config`
- `data`
- `media`

directories.

See [Backups](../deployment/backups.md) for the complete backup procedure.

---

## Why are no observations appearing?

Check that:

- Frigate is running and accessible from WAMF.
- MQTT is configured correctly.
- WAMF is connected to the MQTT broker.
- Camera names in WAMF exactly match the corresponding Frigate camera names.
- Frigate is detecting the configured object type.
- WAMF can access the Frigate HTTP API.

The **Admin Dashboard** and **System Logs** can provide additional information
about Frigate, MQTT and processing problems.

---

## Is WAMF open source?

Yes.

WAMF is an open-source project. The source code is available from the
WAMF Project repository on GitHub.