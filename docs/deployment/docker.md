# Docker

Docker is the recommended way to run WAMF v0.4.2.

The official WAMF container image is published on the GitHub Container Registry:

```text
ghcr.io/wamf-project/wamf:0.4.2
```

WAMF stores its configuration, database and archived media outside the container so that they persist when the container is updated or recreated.

## Requirements

Before installing WAMF, ensure that you have:

- An AMD64 (x86-64) Linux host with Docker installed.
- A working Frigate installation.
- An MQTT broker accessible to both Frigate and WAMF.
- Network access from WAMF to the Frigate HTTP API.
- At least one Frigate camera configured to detect the `bird` object.

See [Frigate Configuration](frigate.md) for the Frigate requirements.

## Create the WAMF Directories

Create a directory for WAMF and its persistent data:

```bash
mkdir -p ~/WAMF/config ~/WAMF/data ~/WAMF/media
cd ~/WAMF
```

These directories are used for:

| Directory | Purpose |
|-----------|---------|
| `config/` | WAMF configuration and authentication settings |
| `data/` | WAMF database |
| `media/` | Archived snapshots and clips |

## Docker Compose

Create a `docker-compose.yml` file in the WAMF directory:

```yaml
services:
  wamf:
    image: ghcr.io/wamf-project/wamf:0.4.2
    container_name: wamf
    restart: unless-stopped

    ports:
      - "7767:7767"

    environment:
      TZ: Europe/London

    volumes:
      - ./config:/app/config
      - ./data:/app/data
      - ./media:/app/media
```

!!! warning "Mount a writable configuration directory"
    Mount the entire `config/` directory, as shown above. Configuration saves
    use locked, atomic replacement; read-only configurations and single-file
    bind mounts cannot support this and fail safely. The container must also
    be able to write to its database and media directories.

Change the `TZ` value if required for your location.

Start WAMF:

```bash
docker compose up -d
```

Check the container:

```bash
docker compose ps
```

To view the logs:

```bash
docker compose logs -f wamf
```

## First Start

On the first start, WAMF automatically creates the files required for initial configuration.

Open the WAMF web interface:

```text
http://<wamf-host>:7767
```

On a fresh installation with administrator authentication enabled, WAMF prints a
private, single-use `/setup#token=...` link to the startup logs:

```bash
docker compose logs wamf
```

Open the link within **15 minutes**, choose and confirm a password of at least
eight characters, then sign in through the normal login page. JavaScript must
be enabled. WAMF no longer prints a temporary administrator password.

If setup expires, restart WAMF to obtain a new link. Restarting while setup is
pending invalidates the previous link; opening or refreshing the page does not
renew it.

!!! warning "Keep the setup link private"
    Anyone with the unused link and network access can set the administrator
    password. Restrict startup and centralized logs to trusted administrators,
    disable proxy request-body capture for setup, and use HTTPS or a trusted
    tunnel for remote password setup.

The printed link uses a loopback address by default. For access from another
machine, replace its host and published port with those of your WAMF deployment,
while retaining the `/setup#token=...` portion. The loopback address is not an
access control. An optional `webui.public_base_url` can supply the external
origin; see [Authentication](../administration/authentication.md).

Existing valid administrator passwords, API credentials and session secrets are
preserved. Provisioned installations receive no setup link. Malformed
authentication configuration stops startup rather than resetting credentials.

Operational workers pause until administrator setup succeeds. They start
automatically when setup completes if Frigate configuration is ready; otherwise
complete the Frigate/MQTT configuration and use **Save & Restart**.

Continue with the [Quick Start](../getting-started/quick-start.md) guide to connect WAMF to Frigate.

## Persistent Data

The container can be stopped, removed, updated or recreated without losing WAMF data as long as the following directories are preserved:

```text
config/
data/
media/
```

Do not store important configuration or media only inside the container filesystem.

See [Backup & Restore](backups.md) for backup recommendations.

## Portainer

WAMF can also be deployed as part of a Portainer stack.

When using Portainer, absolute host paths are recommended for bind mounts.

Example:

```yaml
services:
  wamf:
    image: ghcr.io/wamf-project/wamf:0.4.2
    container_name: wamf
    restart: unless-stopped

    ports:
      - "7767:7767"

    environment:
      TZ: Europe/London

    volumes:
      - /home/your-user/WAMF/config:/app/config
      - /home/your-user/WAMF/data:/app/data
      - /home/your-user/WAMF/media:/app/media
```

Replace `/home/your-user/WAMF` with the location you want to use on the Docker host.

Deploy the stack and then open:

```text
http://<wamf-host>:7767
```

The same first-start setup process applies to both Docker Compose and Portainer deployments.

## File Ownership

WAMF v0.4.2 currently runs as root inside the container.

Files created in bind-mounted `config`, `data` and `media` directories may therefore appear as owned by `root` on the Docker host.

This is expected behaviour in v0.4.2.

Generated configuration files use mode `0600`. Reading or editing them on the
host may require elevated permissions. If using a custom container UID, grant
that user write access to all three mounted directories.

Take file ownership and permissions into account when manually editing, copying or backing up these directories.

## Stopping WAMF

For Docker Compose:

```bash
docker compose down
```

Persistent configuration, database and media files remain in the bind-mounted directories.

To start WAMF again:

```bash
docker compose up -d
```

## Next Steps

Continue to the [Quick Start](../getting-started/quick-start.md) guide to configure WAMF and verify your first detection.