# TaskWise

TaskWise is a self-hosted AI-powered to-do list application.

## Configuration

Before starting the service, ensure you have:
1. Created your `.env.local` file with a valid `GOOGLE_AI_API_KEY` and any other necessary environment variables (see the [TaskWise documentation](https://github.com/zainibeats/taskwise)).
2. (Optional) Adjust the volume paths in `docker-compose.yml` if you want to change where data is stored.

## Setup

1. Navigate to the `taskwise` directory:
    ```bash
    cd taskwise
    ```
2. Start the service using Docker Compose:
    ```bash
    docker-compose up -d
    ```

TaskWise should now be accessible at `http://YOUR_SERVER_IP:3000`.

For full documentation, advanced configuration, and feature details, see the official [TaskWise GitHub repository](https://github.com/zainibeats/taskwise).

