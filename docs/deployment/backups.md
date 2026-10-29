# Backup & Restore

WAMF stores its persistent state in three directories:

```text
config/
data/
media/
```

A complete WAMF backup should include all three.

## What to Back Up

### Configuration

The `config/` directory contains the WAMF configuration and authentication settings.

Back up the entire directory:

```text
config/
```

### Database

The `data/` directory contains the WAMF observation database.

Back up the entire directory:

```text
data/
```

The database contains WAMF observation history and should be included in every complete backup.

### Archived Media

The `media/` directory contains media archived by WAMF.

Back up:

```text
media/
```

The size of this directory will depend on the number of observations and your media retention settings.

Frigate recordings and other media managed directly by Frigate are separate from the WAMF backup.

## Creating a Consistent Backup

For the most consistent backup, stop WAMF before copying its persistent directories.

For Docker Compose:

```bash
docker compose down
```

Copy or archive:

```text
config/
data/
media/
```

Then start WAMF again:

```bash
docker compose up -d
```

For Portainer deployments, stop the WAMF container or stack before taking the backup and start it again afterwards.

## Example Backup

If WAMF is installed in `~/WAMF`, a simple archive can be created with:

```bash
cd ~/WAMF
tar -czf wamf-backup.tar.gz config data media
```

Store the resulting archive somewhere outside the WAMF host if it is intended to protect against disk or host failure.

## Restoring a Backup

Stop WAMF before restoring persistent data.

Restore the backed-up directories to the paths used by your WAMF deployment:

```text
config/
data/
media/
```

Ensure that the Docker bind mounts or Portainer stack point to the restored directories.

Then start WAMF.

For Docker Compose:

```bash
docker compose up -d
```

## Verify the Restore

After restoring a backup:

1. Open the WAMF web interface.
2. Confirm that WAMF starts normally.
3. Check the system health information.
4. Confirm that existing observations are present.
5. Check that archived media is available.
6. Confirm that WAMF reconnects to MQTT and Frigate.
7. Verify that a new detection can be processed.

## File Ownership

WAMF v0.4.3 runs as root inside the Docker container.

Files created in bind-mounted `config`, `data` and `media` directories may therefore be owned by `root` on the Docker host.

When restoring or manually copying WAMF data, ensure that the files remain accessible to the container.

Avoid changing permissions or ownership unnecessarily if the restored installation is already working.

## Before an Update

A complete backup of:

```text
config/
data/
media/
```

is recommended before updating WAMF to another release.

Keep the backup until the updated installation has been verified successfully.

See [Updating WAMF](updating.md) for the update procedure.