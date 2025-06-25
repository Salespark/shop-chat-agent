# Docker Setup for Shop Chat Agent

This document explains how to run the Shop Chat Agent application using Docker Compose.

## Prerequisites

- Docker and Docker Compose installed on your system
- Shopify App credentials (API Key, API Secret)
- Claude API Key for AI functionality

## Quick Start

### 1. Environment Setup

1. Copy the example environment file:
   ```bash
   cp env.example .env
   ```

2. Edit the `.env` file with your actual credentials:
   ```bash
   nano .env
   ```

   **Required variables to update:**
   - `SHOPIFY_API_KEY`: Your Shopify App API Key
   - `SHOPIFY_API_SECRET`: Your Shopify App API Secret
   - `SHOPIFY_APP_URL`: Your app's public URL
   - `REDIRECT_URL`: OAuth callback URL
   - `CLAUDE_API_KEY`: Your Claude API Key

### 2. Build and Run

```bash
# Build and start the container
docker-compose up -d

# View logs
docker-compose logs -f

# Stop the container
docker-compose down
```

## Configuration Options

### Network Configuration

The Docker Compose file supports the following network-related environment variables:

- `HOST_IP`: IP address to bind the container to (default: `0.0.0.0`)
- `PORT`: Port for the main application (default: `3000`)
- `FRONTEND_PORT`: Frontend development port (default: `8002`)
- `HMR_SERVER_PORT`: Hot Module Replacement port (default: `8002`)

### Database Configuration

By default, the application uses SQLite. You can change the database by updating the `DATABASE_URL` in your `.env` file:

**SQLite (default):**
```
DATABASE_URL=file:dev.sqlite
```

**PostgreSQL:**
```
DATABASE_URL=postgresql://username:password@localhost:5432/shop_chat_agent
```

**MySQL:**
```
DATABASE_URL=mysql://username:password@localhost:3306/shop_chat_agent
```

## Volume Mounts

The Docker Compose configuration includes the following volume mounts:

- `./data:/app/data`: Database files and persistent data
- `./logs:/app/logs`: Application logs

## Health Checks

The container includes a health check that monitors the application endpoint. The health check:

- Runs every 30 seconds
- Times out after 10 seconds
- Retries 3 times before marking as unhealthy
- Waits 40 seconds before starting health checks

## Useful Commands

```bash
# Build the image without cache
docker-compose build --no-cache

# Run in foreground (for debugging)
docker-compose up

# View container status
docker-compose ps

# Execute commands inside the container
docker-compose exec shop-chat-agent sh

# View container logs
docker-compose logs shop-chat-agent

# Stop and remove containers, networks, and volumes
docker-compose down -v

# Restart the service
docker-compose restart shop-chat-agent
```

## Troubleshooting

### Container won't start
1. Check if all required environment variables are set in `.env`
2. Verify that the ports are not already in use
3. Check Docker logs: `docker-compose logs shop-chat-agent`

### Database issues
1. Ensure the `data` directory exists and has proper permissions
2. Check if the database file is accessible from within the container
3. Verify the `DATABASE_URL` format

### Network connectivity
1. Ensure the `HOST_IP` is correctly set
2. Check if the port is accessible from your host machine
3. Verify firewall settings

## Production Considerations

For production deployment:

1. **Security**: Use strong, unique passwords and API keys
2. **Database**: Consider using a managed database service instead of SQLite
3. **SSL/TLS**: Set up proper SSL certificates for HTTPS
4. **Monitoring**: Add monitoring and logging solutions
5. **Backup**: Implement regular database backups
6. **Scaling**: Consider using Docker Swarm or Kubernetes for scaling

## Environment Variables Reference

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `HOST_IP` | No | `0.0.0.0` | IP address to bind the container |
| `PORT` | No | `3000` | Application port |
| `NODE_ENV` | No | `production` | Node.js environment |
| `SHOPIFY_API_KEY` | Yes | - | Shopify App API Key |
| `SHOPIFY_API_SECRET` | Yes | - | Shopify App API Secret |
| `SHOPIFY_APP_URL` | Yes | - | App's public URL |
| `REDIRECT_URL` | Yes | - | OAuth callback URL |
| `SCOPES` | No | - | App scopes (comma-separated) |
| `CLAUDE_API_KEY` | Yes | - | Claude API Key |
| `DATABASE_URL` | No | `file:dev.sqlite` | Database connection string | 