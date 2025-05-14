# Arr Stack

This setup is based on the configuration provided by [TechHutTV/homelab](https://github.com/TechHutTV/homelab), customized for personal needs.

## Overview

The "Arr Stack" is a collection of services for managing and automating media downloads and organization. This setup includes:

- **VPN Protection**: All download traffic is routed through a VPN using Gluetun
- **Download Clients**: qBittorrent (torrents) and NZBGet (Usenet)
- **Media Management**: Sonarr, Radarr, Lidarr, and Bazarr
- **Indexer Management**: Prowlarr
- **Additional Tools**: Firefox browser and Homarr dashboard

## Components

| Service | Description | Web UI Port |
|---------|-------------|-------------|
| Gluetun | VPN client container | N/A |
| qBittorrent | Torrent download client | 8123 |
| NZBGet | Usenet download client | 6789 |
| Prowlarr | Indexer management | 9696 |
| Sonarr | TV show management | 8989 |
| Radarr | Movie management | 7878 |
| Lidarr | Music management | 8686 |
| Bazarr | Subtitle management | 6767 |
| Firefox | Browser with VPN protection | 3001 |
| Homarr | Service dashboard | 7575 |
| Deunhealth | Container health monitor | N/A |

## Network Architecture

This stack uses a custom Docker network (`servarrnetwork`) with static IP addresses for each service. The download clients and Prowlarr route their traffic through the Gluetun VPN container for privacy and security.

## Prerequisites

1. Docker and Docker Compose installed
2. A VPN subscription (ProtonVPN, Mullvad, etc.)
3. A dedicated `/data` directory for media storage
4. User permissions set correctly (PUID/PGID)

## Setup Instructions

### 1. Prepare Your Data Directory

Create a structured data directory:

```bash
mkdir -p /data/{books,movies,music,shows,youtube}
mkdir -p /data/downloads/qbittorrent/{completed,incomplete,torrents}
mkdir -p /data/downloads/nzbget/{completed,intermediate,nzb,queue,tmp}
```

### 2. Configure VPN Settings

Edit the `docker-compose.yml` file and update the Gluetun VPN settings:

```yaml
environment:
  - VPN_SERVICE_PROVIDER=protonvpn    # Change to your VPN provider
  - VPN_TYPE=wireguard                # Choose 'wireguard' or 'openvpn'
  - WIREGUARD_PUBLIC_KEY=<PUBLIC_KEY>  # Replace with your WireGuard public key
  - WIREGUARD_PRIVATE_KEY=<PRIVATE_KEY> # Replace with your WireGuard private key
  - SERVER_COUNTRIES=<COUNTRY>         # Comma-separated list of server countries
  - TZ=<YOUR_TIME_ZONE>                # Set your local timezone
```

### 3. Set User Permissions

Update the PUID and PGID values in the `docker-compose.yml` file to match your user:

```bash
# Find your user ID and group ID
id $(whoami)
# Example output: uid=1000(user) gid=1000(user) groups=...
```

Then update all PUID and PGID values in the compose file accordingly.

### 4. Start the Stack

```bash
cd arr-stack
docker compose up -d
```

## Post-Setup Configuration

### qBittorrent

- Default login: username `admin`, password will be shown in logs the first time
- After login, go to Settings > WebUI to change credentials
- Set network interface to `tun0` in Advanced settings to prevent VPN timeout issues

### NZBGet

- Configure your Usenet provider details in Settings
- Under "INCOMING NZBS", set "AppendCategoryDir" to "No" to prevent path mapping issues with Sonarr/Radarr

### Prowlarr

- Add your indexers/trackers
- Configure applications (Sonarr, Radarr, etc.) using their internal Docker network IPs:
  - Sonarr: 172.69.0.3
  - Radarr: 172.69.0.4
  - Lidarr: 172.69.0.5

### Sonarr/Radarr/Lidarr

- Add qBittorrent and NZBGet as download clients
- Configure root folders pointing to `/data/shows`, `/data/movies`, etc.
- Connect to Prowlarr for indexers

### Bazarr

- Connect to Sonarr and Radarr
- Configure subtitle providers

### Homarr

- Access at port 7575
- Add your services to the dashboard

## Troubleshooting

### Testing VPN Connectivity

```bash
docker run --rm --network=container:gluetun alpine:3.18 sh -c "apk add wget && wget -qO- https://ipinfo.io"
```

### qBittorrent Stalls with VPN Timeout

If qBittorrent stalls when the VPN connection drops:

1. In qBittorrent WebUI, go to Advanced settings and set the network interface to `tun0`
2. The `deunhealth` container is already configured to automatically restart qBittorrent if it becomes unhealthy

### Gluetun High RAM Usage

If Gluetun is using excessive RAM, add this environment variable:

```yaml
- BLOCK_MALICIOUS=off
```

### NZBGet "Directory Does Not Appear to Exist" Error

In NZBGet settings, under "INCOMING NZBS", set "AppendCategoryDir" to "No".

## Maintenance

### Updating Containers

To update all containers to their latest versions:

```bash
docker compose pull
docker compose up -d
```

### Backing Up Configurations

All service configurations are stored in the respective subdirectories (./sonarr, ./radarr, etc.). Back these up regularly.

## Security Considerations

- Replace all placeholder values (`<YOUR_TIME_ZONE>`, `<USERNAME>`, `<PASSWORD>`, etc.) with your actual information
- Generate a secure encryption key for Homarr using `openssl rand -hex 32`
- Consider changing default ports for additional security
