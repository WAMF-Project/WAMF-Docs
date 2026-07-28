# Backup & Restore

Regular backups are recommended to protect your observations, configuration and media.

## What should be backed up?

A complete WAMF backup consists of:

- `config/`
- `data/`
- `media/`

These directories contain:

| Directory | Contents |
|-----------|----------|
| `config/` | WAMF configuration and authentication settings. |
| `data/` | SQLite database containing observations and system data. |
| `media/` | Snapshots and video clips. |

## Creating a Backup

Before creating a backup, stop WAMF to ensure the database is in a consistent state.

Create copies of the following directories:

```text
config/
data/
media/
```

Store backups on another disk or external storage whenever possible.

## Restoring a Backup

1. Stop WAMF.
2. Restore the `config`, `data` and `media` directories.
3. Verify file permissions.
4. Start WAMF.

WAMF will continue using the restored configuration and database.

## Recommendations

- Back up regularly.
- Keep multiple backup generations.
- Test your backups occasionally by restoring them to a test system.