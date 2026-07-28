# Media Retention

WAMF includes a built-in retention system to help manage disk usage by automatically removing old data and identifying orphaned media.

Retention policies are configured in `config.yml` and processed by a scheduled retention task.

## What is removed?

Depending on your configuration, the retention task can:

- Remove old snapshots.
- Remove old video clips.
- Remove old system events.
- Identify orphaned media files that are no longer referenced by the database.
- Remove orphaned media files.
- Limit the number of configuration backups retained.

By default, media is **not** deleted until the appropriate delete options are enabled.

## Configuration

Retention is configured in the `retention` section of `config.yml`.

| Setting | Description |
|----------|-------------|
| `enabled` | Enable or disable retention processing. |
| `snapshots_days` | Number of days to retain snapshots. |
| `clips_days` | Number of days to retain video clips. |
| `delete_media` | Delete expired media files. |
| `orphan_scan_enabled` | Scan for orphaned media files. |
| `delete_orphaned_media` | Delete orphaned media files found during a scan. |
| `system_events_days` | Number of days to retain system events. |
| `system_events_min_rows` | Minimum number of system events to retain regardless of age. |
| `config_backups_max_files` | Maximum number of configuration backups to retain. |
| `species_overrides` | Optional per-species retention settings. |


## Scheduling Retention

The retention system is implemented by the `retention.py` script located in the WAMF installation directory.

This script should be executed on a regular schedule. For most installations, running it once each night is sufficient.

On Linux, the recommended approach is to schedule the script using `cron`.

## Example Cron Job

Run the retention task every day at 03:00:

```cron
0 3 * * * cd /opt/wamf && /usr/bin/python3 retention.py >/dev/null 2>&1
```
!!! note

    Adjust the Python executable and WAMF installation path to match your environment.

## Orphaned Media

Occasionally media files may remain on disk even though the corresponding database entry has been removed.

When orphan scanning is enabled, WAMF searches for these files and reports or removes them depending on your configuration.

## Recommendations

For most installations, the default retention settings are suitable.

Consider enabling media deletion only after confirming that your retention policy meets your requirements and that any important observations have been backed up.