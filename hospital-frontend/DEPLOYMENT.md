# Hospital Frontend - Docker Deployment

This project is configured for single-command container deployment using Docker Compose.

## Prerequisites
- Docker Desktop installed and running

## Quick Start (Local)
From the `hospital-frontend` directory:

```powershell
# Build and start in background
docker compose up -d

# App will be available at http://localhost:8081
```

- Host port: `8081` (chosen to avoid Jenkins on 8080)
- Container port: `80` (Nginx)

## Configuration
- Change host port by editing `docker-compose.yml` (e.g., `3000:80`).
- Build-time env for Vite: define in `.env` (optional)

```
VITE_API_BASE_URL=https://api.example.com
```

## Useful Commands
```powershell
# View logs
docker compose logs -f

# Rebuild after code changes
docker compose build --no-cache

# Stop
docker compose down

# Remove volumes/networks if needed
docker compose down -v
```

## Optional: Push Image to Registry
Image name defaults to `balaji30589/hospital-frontend:latest`.

```powershell
docker compose build
docker login
docker compose push
```
