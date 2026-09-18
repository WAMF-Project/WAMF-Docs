# Docker

Docker is the recommended way to run WAMF v0.4.0.

The official WAMF container image is published on the GitHub Container Registry:

```text
ghcr.io/wamf-project/wamf:0.4.0
```

WAMF stores its configuration, database and archived media outside the container so that they persist when the container is updated or recreated.

## Requirements

Before installing WAMF, ensure that you have:

- A Linux host with Docker installed.
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
    image: ghcr.io/wamf-project/wamf:0.4.0
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

If valid administrator credentials do not already exist, WAMF generates an initial administrator password during bootstrap.

The temporary password is displayed in the container logs.

Retrieve it with:

```bash
docker compose logs wamf
```

Sign in using the generated password and change it after logging in.

A new installation starts in setup mode until the required Frigate and MQTT settings have been configured.

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
    image: ghcr.io/wamf-project/wamf:0.4.0
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

WAMF v0.4.0 currently runs as root inside the container.

Files created in bind-mounted `config`, `data` and `media` directories may therefore appear as owned by `root` on the Docker host.

This is expected behaviour in v0.4.0.

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