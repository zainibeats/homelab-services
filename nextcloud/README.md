# Nextcloud

Nextcloud is a suite of client-server software for creating and using file hosting services. It provides functionality similar to Dropbox, Office 365, or Google Drive when used with its integrated office suites Collabora Online or OnlyOffice.

## Configuration

Before starting the service, ensure you have:
1.  Updated the following environment variables in `docker-compose.yml` for the `db` service:
    *   `MYSQL_ROOT_PASSWORD`: Choose a strong password for the MariaDB root user.
    *   `MYSQL_PASSWORD`: Choose a strong password for the Nextcloud database user.
2.  Updated the `MYSQL_PASSWORD` environment variable for the `app` service to match the one set for the `db` service.
3.  Optionally, modify the host port mapping (default is `8080:80`).

## Setup

1.  Navigate to the `nextcloud` directory:
    ```bash
    cd nextcloud
    ```
2.  Start the services using Docker Compose:
    ```bash
    docker-compose up -d
    ```

Nextcloud should now be accessible at `http://<your-server-ip>:8080` (or the custom port you configured). Follow the on-screen setup instructions, using `db` as the database host and the database credentials you set in the `docker-compose.yml`. 
