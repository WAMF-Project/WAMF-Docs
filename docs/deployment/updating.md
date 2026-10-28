# Updating WAMF

WAMF Docker releases are published as versioned images on the GitHub Container Registry.

The current maintenance release is **v0.4.2**, for AMD64 (x86-64) deployments.
See the [v0.4.2 release notes](https://github.com/WAMF-Project/WAMF/blob/v0.4.2/docs/release-0.4.2.md)
for changes and retained security limitations.

## Upgrading v0.4.0 or v0.4.1 to v0.4.2

Existing provisioned installations with valid credentials require no
configuration, database or media migration and no credential reset.

1. Stop WAMF cleanly before taking a backup, for example with
   `docker compose stop wamf`.
2. Back up the complete `config/`, `data/` and `media/` directories together.
3. Keep the previous image locally available for rollback; do not prune it
   before verifying the upgrade.
4. Preserve the existing mounts, port mappings and `WAMF_SECRET_KEY` environment
   value, if set. Retain any additional `/data` or `/media` compatibility mounts
   already used by an older installation.
5. Set the image to `ghcr.io/wamf-project/wamf:0.4.2`, then pull and recreate it
   using the instructions below.

Mount the entire writable configuration directory. Single-file and read-only
configuration mounts cannot support atomic configuration replacement.

Valid administrator passwords, API credentials and session secrets are preserved.
Malformed or unsupported authentication configuration stops startup rather than
resetting credentials. Only a genuinely unprovisioned installation receives a
first-run setup link; see [Authentication](../administration/authentication.md).

Before updating, back up your WAMF configuration, database and media.

See [Backup & Restore](backups.md) for the recommended backup procedure.

## Check the Current Version

If WAMF is running with Docker Compose, check the configured image:

```bash
docker compose config | grep image
```

You can also check the running container with:

```bash
docker ps --filter name=wamf
```

For WAMF v0.4.0, the image is:

```text
ghcr.io/wamf-project/wamf:0.4.0
```

## Back Up WAMF

Before updating, preserve the WAMF persistent directories:

```text
config/
data/
media/
```

These contain the WAMF configuration, database and archived media.

For the most consistent database backup, stop WAMF before copying these directories.

## Update the Image Version

Edit your Docker Compose or Portainer stack and change the WAMF image tag to the version you want to install.

For this upgrade, set:

```yaml
image: ghcr.io/wamf-project/wamf:0.4.2
```

Using an explicit version tag is recommended because it makes upgrades deliberate and makes it clear which WAMF release is running.

## Docker Compose

After changing the image version, pull the new image:

```bash
docker compose pull wamf
```

Recreate the container:

```bash
docker compose up -d
```

Docker Compose will replace the container while retaining the bind-mounted WAMF configuration, database and media directories.

Check the container status:

```bash
docker compose ps
```

Then review the startup logs:

```bash
docker compose logs -f wamf
```

## Portainer

For a Portainer deployment:

1. Open the WAMF stack.
2. Change the WAMF image tag to the required version.
3. Update or redeploy the stack.
4. Allow Portainer to pull the updated image.
5. Check the WAMF container logs after deployment.

The existing bind-mounted `config`, `data` and `media` directories should remain unchanged.

## Verify the Update

After updating:

1. Open the WAMF web interface.
2. Confirm that WAMF starts normally.
3. Check the system health information.
4. Confirm that WAMF connects to MQTT.
5. Confirm that WAMF can communicate with Frigate.
6. Verify that existing observations and archived media are still available.
7. Test or wait for a new bird detection and confirm that it is processed successfully.
8. Confirm that the existing administrator login and API token still work.
9. Restart or recreate the container and verify that credentials, observations
   and archived media persist.

## Rollback

If an update causes a problem, stop WAMF and restore the backup made before the update if required.

Use the same mounts, port mappings and session-secret environment value.
Never run the old and new containers as simultaneous writers to the database.

Change the image tag back to the previous version.

For example:

```yaml
image: ghcr.io/wamf-project/wamf:0.4.0
```

Then pull and recreate the container:

```bash
docker compose pull wamf
docker compose up -d
```

If the newer WAMF release changed persistent data or configuration in a way that is not compatible with the previous release, restore the corresponding pre-update backup before starting the older version.

Always check the release notes for version-specific upgrade or rollback instructions.

## Image Tags

WAMF releases use versioned container tags.

For example:

```text
ghcr.io/wamf-project/wamf:0.4.2
```

The `latest` tag may also be available, but explicit version tags are recommended for production deployments.

This prevents an installation from changing WAMF versions simply because a newer `latest` image has been published.