# homelab-services

This repository contains Docker Compose configurations for various self-hosted services used in my homelab.

## Services

Below is a list of services configured in this repository. Each directory contains the `docker-compose.yml` file and a specific `README.md` with setup instructions.

- **Arr Stack** ([docs](./arr-stack/README.md)): A collection of services for media management including Sonarr, Radarr, Lidarr, and more. Uses NFS for media storage with a unified `/data` directory structure.
- **Nextcloud** ([docs](./nextcloud/README.md)): Self-hosted file sync and share platform with NFS storage integration.
- **Jellyseerr** ([docs](./jellyseerr/README.md)): A request management and media discovery tool for the Jellyfin/Emby ecosystem.
- **Nginx Proxy Manager + DDClient** ([docs](./nginx-ddclient/README.md)): Easy-to-use Nginx proxy manager with dynamic DNS updating via DDClient.
- **Ollama + Open WebUI** ([docs](./ollama-openwebui/README.md)): Run large language models locally with Ollama and interact with them through Open WebUI.
- **Open Source Monitoring** ([docs](./opensource-monitoring/README.md)): Monitoring stack including Prometheus, Grafana, Node Exporter, and cAdvisor.

## Storage Configuration

This homelab is designed with network storage in mind:

- **NFS Shares**:
  - Media: `/mnt/nfs/jellyfin` (for Arr Stack)
  - Nextcloud: `/mnt/nfs/family/nextcloud`
  - Configured in `/etc/fstab` for automatic mounting

- **Local Storage Fallback**:
  - Services can be configured to use local storage if NFS is not available
  - See individual service READMEs for configuration details

## Prerequisites

-   Docker
-   Docker Compose

## Automatic Updates (Optional)

To automatically update your running Docker containers to the latest image, consider using [Watchtower](https://containrrr.dev/watchtower/).

A basic Watchtower container might look like this:

```bash
docker run -d \
--name watchtower \
-v /var/run/docker.sock:/var/run/docker.sock \
containrrr/watchtower
```
