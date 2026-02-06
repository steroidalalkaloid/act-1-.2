# Task Manager API

A REST API built with Symfony and API Platform to manage tasks with full CRUD operations.

## Features
- ✅ Create, read, update, and delete tasks
- ✅ Mark tasks as completed
- ✅ Auto-generated API documentation (Swagger/OpenAPI)
- ✅ MySQL database integration
- ✅ Docker containerization
- ✅ CORS support

## Requirements
- Docker & Docker Compose
- PHP 8.4+ (for local development)

## Quick Start

### 1. Clone/Navigate to Project
```bash
cd task-manager-api
```

### 2. Start Docker Containers
```bash
docker compose up -d --build
```
This starts:
- PHP-FPM application container
- Nginx web server (port 8080)
- MySQL database (port 3306)
- Adminer database manager (port 8081)

### 3. Wait for Database & Install Dependencies
```bash
# Wait 10-15 seconds for MySQL to be ready

# Install composer dependencies
docker compose exec app composer install
```

### 4. Create & Run Database Migrations
```bash
# Generate migration from Task entity
docker compose exec app php bin/console doctrine:migrations:diff

# Run the migration to create tables
docker compose exec app php bin/console doctrine:migrations:migrate --no-interaction
```

## Access the API

### API Documentation (Interactive)
Visit: **http://localhost:8080/api**

This shows the Swagger UI with full API documentation and the ability to test endpoints directly.

### Database Manager (Adminer)
Visit: **http://localhost:8081**
- Server: `db`
- Username: `symfony`
- Password: `secret`
- Database: `task_manager_api`

## API Endpoints

All endpoints are prefixed with `/api`

### List all tasks
```
GET /api/tasks
```

### Create a new task
```
POST /api/tasks
Content-Type: application/json

{
  "title": "Buy groceries",
  "description": "Milk, eggs, bread",
  "isCompleted": false
}
```

### Get a specific task
```
GET /api/tasks/{id}
```

### Update a task (PUT - replace all fields)
```
PUT /api/tasks/{id}
Content-Type: application/json

{
  "title": "Buy groceries",
  "description": "Updated description",
  "isCompleted": false
}
```

### Partial update (PATCH - update specific fields)
```
PATCH /api/tasks/{id}
Content-Type: application/json

{
  "isCompleted": true
}
```

### Mark a task as completed
```
PATCH /api/tasks/{id}
Content-Type: application/json

{
  "isCompleted": true
}
```

### Delete a task
```
DELETE /api/tasks/{id}
```

## Database Schema

### Task Table
| Field | Type | Default | Notes |
|-------|------|---------|-------|
| id | INT | AUTO_INCREMENT | Primary key |
| title | VARCHAR(255) | - | Required |
| description | TEXT | NULL | Optional |
| isCompleted | BOOLEAN | false | Default false |
| createdAt | DATETIME | NOW() | Auto-set on creation |

## Project Structure
```
task-manager-api/
├── config/              # Symfony configuration
│   ├── packages/       # Bundle configurations
│   └── routes/         # API routing
├── src/
│   ├── Entity/         # Task entity
│   ├── Controller/     # API controllers (if needed)
│   └── Kernel.php      # Application kernel
├── migrations/         # Database migrations
├── docker/            # Docker configurations
│   └── nginx/         # Nginx config
├── docker-compose.yml # Docker Compose setup
├── Dockerfile         # PHP-FPM image
└── public/           # Web root
```

## Troubleshooting

### Containers won't start
```bash
# Check Docker logs
docker compose logs app
docker compose logs db

# Rebuild containers
docker compose down -v
docker compose up -d --build
```

### Database connection error
- Wait 15 seconds after `docker compose up` for MySQL to be ready
- Check `.env` DATABASE_URL is set correctly

### Migration errors
```bash
# Check current migration status
docker compose exec app php bin/console doctrine:migrations:status

# View generated migration
ls migrations/
```

### Clear cache
```bash
docker compose exec app php bin/console cache:clear
```

## Notes
- Database name: `task_manager_api`
- Default `isCompleted` value is `false`
- `createdAt` timestamp is automatically set when a task is created
- All API responses are in JSON format
- CORS is enabled for local development

## Development

### Stop containers
```bash
docker compose down
```

### Stop and remove all data
```bash
docker compose down -v
```

### View logs
```bash
docker compose logs -f app
```

### Execute Symfony commands
```bash
docker compose exec app php bin/console [command]
```
