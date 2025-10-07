# Media Services

This directory contains services for managing, streaming, and requesting media content in the homelab.

## Services Overview

### Media Management & Automation
- **[Arr Stack](./arr-stack/README.md)** - Complete media automation suite including:
  - **Sonarr** - TV show management and automation
  - **Radarr** - Movie management and automation
  - **Lidarr** - Music management and automation
  - **Bazarr** - Subtitle management
  - **Prowlarr** - Indexer management for all *arr services
  - **qBittorrent** - Torrent download client (VPN-protected)
  - **NZBGet** - Usenet download client (VPN-protected)
  - **Gluetun** - VPN client for securing download traffic
  - **Jellyseerr** - Media request management and discovery
  - **Homarr** - Service dashboard
  - **Firefox** - Browser with VPN protection

### Media Streaming
- **[Jellyfin](./jellyfin/README.md)** - Self-hosted media server for streaming movies, TV shows, music, and more

## Common Considerations

### Network Storage
Media services are designed to work with network storage (NFS/SMB) for efficient storage management:
- **TrueNAS Dataset**: `jellyfin_data/`
- **NFS Mount**: `/mnt/nfs/jellyfin`
- See individual service READMEs for specific configuration details

### VPN Protection
All download traffic in the Arr Stack is routed through a VPN using Gluetun to ensure privacy and security.

### Media Workflow
1. **Request**: Users request content via Jellyseerr
2. **Search**: Sonarr/Radarr search for content via Prowlarr indexers
3. **Download**: Content is downloaded via qBittorrent or NZBGet through VPN
4. **Process**: Media is automatically renamed, organized, and moved to appropriate folders
5. **Subtitles**: Bazarr automatically downloads subtitles
6. **Stream**: Content becomes available in Jellyfin for streaming

### Integration
- Jellyseerr integrates with Sonarr/Radarr for automated content requests
- Jellyfin scans the media library for new content
- All services communicate via Docker networking for seamless automation
