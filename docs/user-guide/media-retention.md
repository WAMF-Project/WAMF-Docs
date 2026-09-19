# Media Retention

WAMF includes a retention system to help manage stored media, system events
and configuration backups.

Retention policies are configured in `config.yml` and applied when the
retention task runs.

!!! important
    Enabling retention in `config.yml` does not schedule the retention task.
    The task must also be scheduled to run periodically.

## What Does Retention Manage?

Depending on the configuration, the retention task can:

- process old snapshots
- process old video clips
- remove old system events
- identify orphaned media files that are no longer referenced by the database
- remove orphaned media files
- limit the number of configuration backups retained
- apply different media retention periods to individual species

Media deletion is controlled separately, allowing retention to be configured
without immediately removing files.

## Configuration

Retention is configured in the `retention` section of `config.yml`.

| Setting | Description |
|----------|-------------|
| `enabled` | Enable retention processing when the retention task is run. |
| `snapshots_days` | Number of days to retain snapshots. |
| `clips_days` | Number of days to retain video clips. |
| `delete_media` | Delete media files that have exceeded their configured retention period. |
| `orphan_scan_enabled` | Scan for orphaned media files. |
| `delete_orphaned_media` | Delete orphaned media files found during a scan. |
| `system_events_days` | Number of days to retain system events. |
| `system_events_min_rows` | Minimum number of system events to retain regardless of age. |
| `config_backups_max_files` | Maximum number of configuration backups to retain. |
| `species_overrides` | Optional per-species media retention periods. |

See the [Configuration Reference](../reference/configuration.md) for the
complete configuration syntax and defaults.

## Species-Specific Retention

Different retention periods can be configured for individual species using
`species_overrides`.

For example:

```yaml
retention:
  species_overrides:
    Erithacus rubecula:
      snapshots_days: 730
      clips_days: 180
```

This example retains snapshots and clips for the European Robin for longer
than the standard retention period.

Species overrides use the scientific species name.

## Running Retention

Retention processing is performed by the `retention.py` script included with
WAMF.

It is recommended to run the task manually before scheduling it. This confirms
that retention can access the WAMF configuration, database and media storage
correctly.

### Docker Installation

For the recommended Docker installation, run the retention task inside the
running WAMF container:

```bash
docker exec wamf python retention.py
```

A successful run produces output similar to:

```text
INFO [__main__] Retention scan complete
INFO [__main__] Scanned rows: 19
INFO [__main__] Orphan scan complete
INFO [__main__] Orphans found: 0
INFO [__main__] Missing files: 0
```

The numbers will vary depending on the contents of your WAMF installation.

Review the output for errors before scheduling the task.

## Scheduling Docker Retention

On a Linux Docker host, the retention task can be scheduled using `cron`.

First confirm the location of Docker:

```bash
command -v docker
```

A typical installation returns:

```text
/usr/bin/docker
```

Open the current user's crontab:

```bash
crontab -e
```

To run retention every day at 03:00, add:

```cron
0 3 * * * /usr/bin/docker exec wamf python retention.py >/dev/null 2>&1
```

!!! note
    If `command -v docker` returned a different path, use that path in the
    cron entry instead of `/usr/bin/docker`.

Save and close the crontab.

Confirm that the job has been installed:

```bash
crontab -l
```

The retention task will now run once each day at 03:00.

!!! note
    The Docker command assumes the WAMF container is named `wamf`, as used by
    the standard WAMF Docker installation. If you have changed the container
    name, use your container name instead.

## Native Installation

For a native WAMF installation, `retention.py` can also be run directly using
the Python environment used by WAMF.

The exact command depends on the installation path and Python virtual
environment.

For example:

```bash
cd /path/to/WAMF
.venv/bin/python retention.py
```

!!! note
    Native WAMF installations require Python 3.11. Use the same Python
    environment used to run WAMF itself.

The native retention command should be tested manually before adding it to
`cron`.

## Orphaned Media

An orphaned media file is a snapshot or clip that remains on disk without a
corresponding database reference.

When `orphan_scan_enabled` is enabled, the retention task scans for orphaned
media.

The `delete_orphaned_media` setting controls whether files identified by the
scan are removed.

Keeping scanning and deletion as separate options allows orphaned files to be
identified without automatically deleting them.

## Configuration Backups

WAMF creates a backup of the previous configuration before saving a
configuration change.

The `config_backups_max_files` retention setting controls the maximum number
of these configuration backups retained.

This is separate from backing up the complete WAMF installation.

See [Backups](../deployment/backups.md) for information about protecting the
WAMF configuration, database and archived media.

## Checking Retention

The WAMF **Admin Dashboard** reports the status of the retention system.

Retention-related events can also be reviewed under **Admin → Logs**.

After initially configuring retention, check these areas after the scheduled
task has run to confirm that retention is operating normally.

## Before Enabling Media Deletion

It is recommended to run retention with media deletion disabled initially.

This allows the retention task and orphan scanning to be tested before WAMF
is allowed to remove media files.

Once you are satisfied that the configured retention periods match your
requirements, media deletion can be enabled using:

```yaml
retention:
  delete_media: true
```

Orphaned media deletion is controlled separately:

```yaml
retention:
  delete_orphaned_media: true
```

!!! warning
    Enabling these options allows the retention task to permanently remove
    media files.

    Important observations or media that need to be preserved should be
    included in your normal WAMF backups before enabling automatic deletion.