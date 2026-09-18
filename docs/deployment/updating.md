# Updating WAMF

WAMF Docker releases are published as versioned images on the GitHub Container Registry.

Before updating, back up your WAMF configuration and data.

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

For example:

```yaml
image: ghcr.io/wamf-project/wamf:0.4.0
```

should be changed to the new version when one is released.

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

## Rollback

If an update causes a problem, stop WAMF and restore the backup made before the update if required.

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
ghcr.io/wamf-project/wamf:0.4.0
```

The `latest` tag may also be available, but explicit version tags are recommended for production deployments.

This prevents an installation from changing WAMF versions simply because a newer `latest` image has been published.