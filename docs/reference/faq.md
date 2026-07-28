# Frequently Asked Questions

## Does WAMF replace Frigate?

No.

Frigate performs object detection.

WAMF receives Frigate events and adds wildlife-specific classification, storage, analytics and management.

---

## Does WAMF perform object detection?

No.

Object detection is currently provided by Frigate.

---

## Does WAMF require an Internet connection?

No.

WAMF is designed to run entirely on your local network.

An Internet connection is only required for optional features such as species metadata.

---

## Can WAMF monitor multiple cameras?

Yes.

Multiple Frigate cameras can be configured in `config.yml`.

---

## Can WAMF identify animals other than birds?

The current release is primarily designed for bird monitoring.

Support for additional wildlife may be added in future releases.

---

## Where are observations stored?

Observations are stored in the WAMF SQLite database located in the `data` directory.

Associated media is stored in the `media` directory.

---

## Can I back up my observations?

Yes.

Back up the `config`, `data` and `media` directories.

See the **Backup & Restore** guide for more information.

---

## Why are no observations appearing?

Check that:

- Frigate is running.
- MQTT is configured correctly.
- Camera names match your Frigate configuration.
- The configured object type is being detected.
- WAMF can access the Frigate API.

---

## Is WAMF open source?

Yes.

WAMF is released as an open-source project.