# homelab-services

This repository contains Docker Compose configurations for various self-hosted services used in my homelab.

## Services

Below is a list of services configured in this repository. Each directory contains the `docker-compose.yml` file and a specific `README.md` with setup instructions.

-   [Homarr](./homarr/README.md): A simple, modern dashboard for your server.
-   [Jellyseerr](./jellyseerr/README.md): A request management and media discovery tool for the Jellyfin/Emby ecosystem.
-   [Nextcloud](./nextcloud/README.md): A suite of client-server software for creating and using file hosting services.
-   [Nginx Proxy Manager + DDClient](./nginx-ddclient/README.md): Easy-to-use Nginx proxy manager with dynamic DNS updating via DDClient.
-   [Ollama + Open WebUI](./ollama-openwebui/README.md): Run large language models locally with Ollama and interact with them through Open WebUI.
-   [Open Source Monitoring](./opensource-monitoring/README.md): Monitoring stack including Prometheus, Grafana, Node Exporter, and cAdvisor.

## Prerequisites

-   Docker
-   Docker Compose

## Automatic Updates (Optional)

To automatically update your running Docker containers to the latest image, consider using [Watchtower](https://containrrr.dev/watchtower/). You can add it as another service to your `docker-compose.yml` files.

A basic Watchtower service definition might look like this:

```yaml
services:
  watchtower:
    image: containrrr/watchtower
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    restart: unless-stopped
    command: --cleanup --interval 86400 # Check daily and remove old images
  
  # ... your other services
```

Remember to adjust the command flags (like `--interval`) according to your needs.
