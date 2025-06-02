# ConvertX

ConvertX is a simple yet powerful file conversion service that allows you to convert between various file formats. It provides a web interface for easy file conversion.

## Configuration

Before starting the service, ensure you have:

1. Set a secure `JWT_SECRET` environment variable in `docker-compose.yml` for authentication.

## Setup

1. Navigate to the `convertx` directory:

   ```bash
   cd convertx
   ```

2. Start the service using Docker Compose:

   ```bash
   docker-compose up -d
   ```

ConvertX should now be accessible at `http://<your-server-ip>:3000`.

## Volumes

- `./data:/app/data` - Stores converted files and application data

## Ports

- `3000` - Web interface and API