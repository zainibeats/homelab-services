# homelab-services

This repository contains Docker Compose configurations for various self-hosted services used in my first homelab. The infrastructure is distributed across multiple dedicated machines for isolation (and lack of resources):

## 🖥️ Infrastructure Overview

| Device | Purpose | OS | Key Services |
|--------|---------|----|--------------|
| **TrueNAS Server** | Centralized storage and backups | TrueNAS Scale | ZFS storage, SMB/NFS shares |
| **Ubuntu Server** | Primary application hosting | Ubuntu Server 22.04 LTS | Docker containers, application services |
| **Raspberry Pi** | Network and home automation | Raspberry Pi OS Lite | Pi-Hole, Home Assistant, Taskwise |
| **Debian Server** | Network services and monitoring | Debian 12 (headless) | Nginx Proxy Manager, DDClient, Grafana, Prometheus |

## Services

Below is a list of services configured in this repository. Each directory contains the `docker-compose.yml` file and a specific `README.md` with setup instructions.

- **Arr Stack** ([docs](./arr-stack/README.md)): A collection of services for media management including Sonarr, Radarr, Lidarr, and more. Uses NFS for media storage with a unified `/data` directory structure.
- **ConvertX** ([docs](./convertx/README.md)): Simple file conversion service with a web interface.
- **Home Assistant** ([docs](./home-assistant/README.md)): Home automation platform.
- **Immich** ([docs](./immich/README.md)): Self-hosted photo and video management platform.
- **Jellyseerr** ([docs](./jellyseerr/README.md)): A request management and media discovery tool for the Jellyfin/Emby ecosystem.
- **Nextcloud** ([docs](./nextcloud/README.md)): Self-hosted file sync and share platform with NFS storage integration.
- **Nginx Proxy Manager + DDClient** ([docs](./nginx-ddclient/README.md)): Easy-to-use Nginx proxy manager with dynamic DNS updating via DDClient.
- **Ollama + Open WebUI** ([docs](./ollama-openwebui/README.md)): Run large language models locally with Ollama and interact with them through Open WebUI.
- **Open Source Monitoring** ([docs](./opensource-monitoring/README.md)): Monitoring stack including Prometheus, Grafana, Node Exporter, and cAdvisor.
- **Syncthing** ([docs](./syncthing/README.md)): Continuous file synchronization program that synchronizes files between two or more computers in real time.
- **Vaultwarden** ([docs](./vaultwarden/README.md)): Self-hosted password manager. Compatible with the official [Bitwarden](https://bitwarden.com/) app.

## Storage Configuration

This homelab is designed with network storage in mind:

- **TrueNAS Datasets and Services**:
  - `/mnt/Ironwolf-Pro-8TB-Mirror/` (Main Storage Pool)
    - `encrypted/` (Encrypted Dataset)
      - **Services**:
        - vaultwarden
        - syncthing
    - `family/` (Family Dataset)
      - **Service**: nextcloud
    - `immich/` (Immich Dataset)
      - **Service**: immich
    - `jellyfin_data/` (Jellyfin Dataset)
      - **Services**:
        - jellyfin
        - arr

- **NFS Shares**:
  - Media: `/mnt/nfs/jellyfin` (for Arr Stack)
  - Nextcloud: `/mnt/nfs/family/nextcloud`
  - Configured in `/etc/fstab` for automatic mounting
  
- **SMB Shares**:
  - Immich: `/mnt/truenas_data/immich`
  - Jellyfin: `/mnt/truenas_data/jellyfin`

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
