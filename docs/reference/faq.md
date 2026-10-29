# Frequently Asked Questions

## Does WAMF replace Frigate?

No.

In WAMF v0.4.3, Frigate provides object detection and detection events.

WAMF receives those events and adds wildlife-specific classification,
observation storage, media archiving, analytics and management.

---

## Does WAMF perform object detection?

Not in the current release.

WAMF v0.4.3 uses detection events provided by Frigate. WAMF then performs
additional wildlife classification on those detections.

---

## Does WAMF require an Internet connection?

Core detection processing can operate on the local network without Internet
access. Retrieving species metadata requires Internet access.

In v0.4.3, the Monaco configuration editor and its workers are served locally.
Bootstrap and jQuery still load from external CDNs, so a fully functioning web
interface should not be assumed on a disconnected browser. Their integrity checks
verify content; they do not make the CDNs available offline. See
[Security](security.md#browser-assets).

---

## Can WAMF monitor multiple cameras?

Yes.

Multiple Frigate cameras can be configured in `config.yml`.

Camera names configured in WAMF must match the corresponding Frigate camera
names exactly.

See the [Configuration Reference](configuration.md) for details.

---

## Can WAMF identify animals other than birds?

WAMF v0.4.3 is primarily designed for bird monitoring.

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

- Administrator setup has completed; operational workers pause until it succeeds.
- Frigate is running and accessible from WAMF.
- MQTT is configured correctly.
- WAMF is connected to the MQTT broker.
- Camera names in WAMF exactly match the corresponding Frigate camera names.
- Frigate is detecting the configured object type.
- WAMF can access the Frigate HTTP API.

Wait for `MQTT event subscription ready` before testing a new live event.
See the [Quick Start troubleshooting steps](../getting-started/quick-start.md#troubleshooting).

The **Admin Dashboard** and **System Logs** can provide additional information
about Frigate, MQTT and processing problems.

---

## Why has my setup link stopped working?

The first-run link expires after 15 minutes and can be used once. Restarting
while setup is pending invalidates the old link and prints a new one in the
startup logs. Refreshing the page cannot renew it. An already provisioned
installation receives no setup link and should use its existing password.

Keep the link private and use HTTPS or a trusted tunnel for remote setup.
See [Authentication](../administration/authentication.md).

---

## Why are several users affected by login throttling?

Clients behind the same reverse proxy or NAT share the connection peer seen by
WAMF and therefore its login-failure bucket. Forwarding headers do not override
this identity. See [Login throttling](../administration/authentication.md#login-throttling).

---

## Is WAMF v0.4.3 free of known vulnerabilities?

No. It fixes specific security issues but retains documented compatibility and
deployment risks, including the protobuf deferral. See
[Security](security.md) for the scope and operational conditions.

---

## Is WAMF open source?

Yes.

WAMF is an open-source project. The source code is available from the
WAMF Project repository on GitHub.