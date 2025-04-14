# Jellyseerr

Jellyseerr is a free and open source software application for managing requests for your media library. It is a fork of Overseerr built to support Jellyfin and Emby media servers.

## Configuration

Before starting the service, ensure you have:
1.  Updated the `TZ` (Timezone) environment variable in `docker-compose.yml` to your local timezone (e.g., `America/New_York`).
2.  Created the `/srv/appdata/jellyseerr` directory on your host machine or updated the volume path in `docker-compose.yml`.

## Setup

1.  Navigate to the `jellyseerr` directory:
    ```bash
    cd jellyseerr
    ```
2.  Start the service using Docker Compose:
    ```bash
    docker-compose up -d
    ```

Jellyseerr should now be accessible at `http://<your-server-ip>:5055`. 
