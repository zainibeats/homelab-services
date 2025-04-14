# Homarr

Homarr is a simple, modern dashboard that puts all of your apps and services at your fingertips.

## Configuration

Before starting the service, ensure you have:
1.  Created the `./homarr/appdata` directory or updated the volume path in `docker-compose.yml`.
2.  Generated a 64-character hex string for the `SECRET_ENCRYPTION_KEY` environment variable. You can generate one using:
    ```bash
    openssl rand -hex 32
    ```
3.  Updated the `SECRET_ENCRYPTION_KEY` in the `docker-compose.yml` file.

## Setup

1.  Navigate to the `homarr` directory:
    ```bash
    cd homarr
    ```
2.  Start the service using Docker Compose:
    ```bash
    docker-compose up -d
    ```

Homarr should now be accessible at `http://<your-server-ip>:7575`. 
