# Docker Setup

## Prerequisites
- Docker and Docker Compose installed
- All submodules properly cloned (see README.md Cloning section)

## Quick Start

1. **Copy environment file:**
   ```bash
   cp .env.example .env
   ```

2. **Start all services:**
   ```bash
   docker-compose up
   ```
   
   > Note: First run will take time as it installs dependencies and applies database migrations automatically

4. **Access the application:**
   - Frontend: http://localhost:3001
   - Backend API: http://localhost:3000
   - PostgreSQL: localhost:5432
   - Redis: localhost:6379

## Services

The Docker setup includes:
- **Frontend** (Nuxt.js): Port 3001
- **Backend** (Express.js + TypeScript): Port 3000
- **PostgreSQL**: Port 5432 (database)
- **Redis**: Port 6379 (caching and real-time features)

## Development Commands

```bash
# Start services in background
docker-compose up -d

# View logs
docker-compose logs -f [service-name]

# Stop services
docker-compose down

# Restart specific service (if code changes)
docker-compose restart [service-name]

# Reset database (WARNING: destroys all data)
docker-compose down -v
docker-compose up

# Clean up node_modules (if dependencies change)
docker-compose down
docker volume rm task-mgr_be_node_modules task-mgr_fe_node_modules
docker-compose up
```

## Environment Variables

Copy `.env.example` to `.env` and adjust values as needed:

```bash
# Database
DATABASE_URL=postgresql://taskuser:taskpass@localhost:5432/taskmanager

# Redis
REDIS_URL=redis://localhost:6379

# Backend
PORT=3000
NODE_ENV=development

# Frontend
API_URL=http://localhost:3000/api/v1
```
