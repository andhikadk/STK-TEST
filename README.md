# STK Test - Fullstack Menu Tree System

A fullstack application that implements a hierarchical menu tree system with CRUD operations, drag-and-drop functionality, and unlimited depth nesting capability.

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
  - [Local Development (Without Docker)](#local-development-without-docker)
  - [Docker Development Mode](#docker-development-mode)
  - [Docker Production Mode](#docker-production-mode)
- [API Documentation](#api-documentation)
- [Database Schema](#database-schema)
- [Testing](#testing)
- [Project Structure](#project-structure)
- [Environment Variables](#environment-variables)
- [Technology Choices & Architecture Decisions](#technology-choices--architecture-decisions)

---

## Overview

This project is a fullstack menu management system that allows users to create, read, update, and delete hierarchical menu items with parent-child relationships. The system supports unlimited nesting depth and provides drag-and-drop functionality for intuitive menu reorganization.

### Live Demo

Backend API: `http://localhost:4000`
Frontend UI: `http://localhost:3000`
Swagger API Docs: `http://localhost:4000/swagger`

---

## Features

### Backend Features
- ✅ RESTful API for menu management (CRUD operations)
- ✅ Hierarchical data structure with parent-child relationships
- ✅ Unlimited nesting depth support
- ✅ Move menu items between different parent nodes
- ✅ Reorder menu items within the same level
- ✅ Input validation and comprehensive error handling
- ✅ PostgreSQL database with migration system
- ✅ Swagger documentation
- ✅ Health check endpoint
- ✅ CORS configuration
- ✅ Structured logging

### Frontend Features
- ✅ Display menu tree in hierarchical structure
- ✅ Add new menu items at any level
- ✅ Edit existing menu items (inline or modal)
- ✅ Delete menu items with confirmation
- ✅ Drag-and-drop functionality for reordering and restructuring
- ✅ Expand/collapse nested menu items
- ✅ Loading states and error handling
- ✅ Responsive design for mobile and desktop
- ✅ Toast notifications for user feedback
- ✅ Context API for state management

### Bonus Features
- ✅ Docker support (development and production)
- ✅ Multi-stage Docker builds
- ✅ Hot-reload in development mode
- ✅ Comprehensive unit tests (backend)
- ✅ Database seeding functionality
- ✅ Migration management system

---

## Tech Stack

### Backend
- **Language**: Go 1.25
- **Framework**: Fiber v2 (Express-inspired web framework)
- **Database**: PostgreSQL 17
- **ORM**: GORM v1.31.0
- **API Documentation**: Swagger
- **Testing**: Go testing package with testutil helpers
- **Hot Reload**: Air (development mode)

### Frontend
- **Framework**: Next.js 16.0.1
- **Language**: TypeScript 5
- **UI Library**: React 19.2.0
- **Styling**: Tailwind CSS v4
- **State Management**: Context API
- **Drag & Drop**: react-dnd v16
- **Icons**: Lucide React
- **Notifications**: Sonner

### DevOps
- **Containerization**: Docker & Docker Compose
- **Database**: PostgreSQL 17 Alpine
- **Reverse Proxy**: None (direct access)

---

## Architecture

### Backend Architecture

```
Clean Architecture Pattern
├── handlers/     - HTTP request handlers (controllers)
├── services/     - Business logic layer
├── models/       - Data models (GORM entities)
├── dto/          - Data Transfer Objects with validation
├── database/     - Database connection and migrations
├── routes/       - API route definitions
├── middleware/   - Custom middleware (error handling, CORS)
└── utils/        - Utility functions (logging, helpers)
```

### Frontend Architecture

```
Next.js App Router Structure
├── app/          - Pages and layouts (App Router)
├── components/   - Reusable UI components
│   ├── ui/       - Base UI components (Button, Modal, etc.)
│   ├── menu/     - Menu-specific components
│   ├── sidebar/  - Sidebar navigation
│   └── layout/   - Layout components
├── contexts/     - React Context providers
├── lib/          - API client and utilities
└── types/        - TypeScript type definitions
```

### Data Flow

```
Frontend (Next.js)
    ↓ HTTP Requests
Backend API (Fiber)
    ↓ Service Layer
Database (PostgreSQL)
```

---

## Prerequisites

### For Local Development
- **Go** 1.25 or higher
- **Node.js** 20.x or higher
- **PostgreSQL** 17 or higher
- **npm** or **yarn** package manager
- **Make** (optional, for using Makefile commands)

### For Docker Development
- **Docker** 20.x or higher
- **Docker Compose** v2.x or higher

---

## Getting Started

### Local Development (Without Docker)

#### 1. Clone the Repository

```bash
git clone <repository-url>
cd STK-TEST
```

#### 2. Backend Setup

```bash
# Navigate to backend directory
cd backend

# Install dependencies
go mod download

# Copy environment variables
cp .env.example .env

# Edit .env file and configure your local PostgreSQL
# Set DB_HOST=localhost and DB_PORT=5432 for local PostgreSQL
nano .env

# Run migrations
go run main.go -migrate=sql

# (Optional) Seed database with sample data
go run main.go -seed

# Run the backend server
go run main.go

# OR use Makefile commands
make install-deps
make migrate-sql
make seed
make run
```

Backend will be available at `http://localhost:4000`

#### 3. Frontend Setup

```bash
# Navigate to frontend directory (in new terminal)
cd frontend

# Install dependencies
npm install

# Copy environment variables
cp .env.example .env

# Edit .env if needed (default: http://localhost:4000)
nano .env

# Run the development server
npm run dev
```

Frontend will be available at `http://localhost:3000`

---

### Docker Development Mode

#### Start All Services with Hot Reload

```bash
# Backend with hot reload
cd backend
make docker-dev

# Frontend with hot reload (in new terminal)
cd frontend
docker-compose up -d

# OR manually
docker compose -f docker-compose.dev.yml up -d
```

#### Run Migrations and Seeding in Docker

```bash
# Enter backend container
docker exec -it stk_test_api_dev sh

# Run migrations
./app -migrate=sql

# Seed database
./app -seed

# Exit container
exit
```

#### View Logs

```bash
# Backend logs
cd backend
make docker-dev-logs

# Frontend logs
cd frontend
docker-compose logs -f
```

#### Stop Development Containers

```bash
# Backend
cd backend
make docker-dev-down

# Frontend
cd frontend
docker-compose down
```

---

### Docker Production Mode

#### Build and Run All Services

```bash
# Backend (includes PostgreSQL)
cd backend
make docker-up

# Wait for backend to be ready, then start frontend
cd frontend
docker-compose up -d
```

#### Run Initial Setup (First Time Only)

```bash
# Run migrations in production container
docker exec -it stk_test_api_app ./app -migrate=sql

# (Optional) Seed data
docker exec -it stk_test_api_app ./app -seed
```

#### Access Services

- Backend API: `http://localhost:4000`
- Frontend UI: `http://localhost:3000`
- PostgreSQL: `localhost:6543` (mapped from container 5432)

#### Stop Production Containers

```bash
# Backend
cd backend
make docker-down

# Frontend
cd frontend
docker-compose down
```

#### Reset Everything (Remove all data)

```bash
# Backend (removes database volumes)
cd backend
make docker-reset

# Frontend
cd frontend
docker-compose down -v
```

---

## API Documentation

### Swagger UI

Access interactive API documentation at: **http://localhost:4000/swagger**

### API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/health` | Health check endpoint |
| `GET` | `/api/menus` | Get all menus (tree structure) |
| `GET` | `/api/menus/:id` | Get single menu by ID |
| `POST` | `/api/menus` | Create new menu item |
| `PUT` | `/api/menus/:id` | Update menu item |
| `DELETE` | `/api/menus/:id` | Delete menu item (and children) |
| `PATCH` | `/api/menus/:id/move` | Move menu to different parent |
| `PATCH` | `/api/menus/:id/reorder` | Reorder menu within same level |

### Request/Response Examples

#### Create Menu

```bash
POST /api/menus
Content-Type: application/json

{
  "title": "Dashboard",
  "path": "/dashboard",
  "icon": "icon-dashboard",
  "parent_id": null,
  "order_index": 0
}
```

#### Response

```json
{
  "status": 201,
  "message": "Menu created successfully",
  "data": {
    "id": "123e4567-e89b-12d3-a456-426614174000",
    "title": "Dashboard",
    "path": "/dashboard",
    "icon": "icon-dashboard",
    "parent_id": null,
    "order_index": 0,
    "created_at": "2025-11-10T08:00:00Z",
    "updated_at": "2025-11-10T08:00:00Z"
  }
}
```

#### Move Menu

```bash
PATCH /api/menus/{id}/move
Content-Type: application/json

{
  "parent_id": "456e7890-e89b-12d3-a456-426614174000"
}
```

#### Reorder Menu

```bash
PATCH /api/menus/{id}/reorder
Content-Type: application/json

{
  "new_index": 2,
  "old_index": 0
}
```

---

## Database Schema

### Menus Table

```sql
CREATE TABLE menus (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    parent_id UUID REFERENCES menus(id) ON DELETE CASCADE,
    title VARCHAR(255) NOT NULL,
    path VARCHAR(255),
    icon VARCHAR(100),
    order_index INTEGER NOT NULL DEFAULT 0,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    deleted_at TIMESTAMP NULL
);

-- Indexes
CREATE INDEX idx_menus_parent_id ON menus(parent_id);
CREATE INDEX idx_menus_order_index ON menus(order_index);
CREATE INDEX idx_menus_deleted_at ON menus(deleted_at);
CREATE INDEX idx_menus_parent_order ON menus(parent_id, order_index) WHERE deleted_at IS NULL;
```

### Relationships

- **Self-referencing**: `parent_id` references `id` in the same table
- **Cascade Delete**: Deleting a parent automatically deletes all children
- **Soft Delete**: Uses `deleted_at` for soft deletion (GORM feature)

---

## Testing

### Backend Tests

```bash
cd backend

# Run all tests
make test

# Run tests with coverage
make test-coverage

# Or using go directly
go test -v ./...
go test -v -coverprofile=coverage.out ./...
go tool cover -html=coverage.out
```

### Test Coverage

- **Handlers**: Comprehensive unit tests (966 lines)
- **Services**: Business logic testing
- **Database**: Test utilities with in-memory SQLite

### Running Specific Tests

```bash
# Test menu handlers only
go test -v ./internal/handlers -run TestMenu

# Test with verbose output
go test -v -run TestCreateMenu ./internal/handlers
```

---

## Project Structure

```
STK-TEST/
├── backend/
│   ├── config/               # Configuration management
│   ├── docs/                 # Swagger documentation
│   ├── internal/
│   │   ├── database/        # Database connection & migrations
│   │   ├── dto/             # Data Transfer Objects
│   │   ├── handlers/        # HTTP request handlers
│   │   ├── middleware/      # Custom middleware
│   │   ├── models/          # Data models
│   │   ├── routes/          # Route definitions
│   │   ├── services/        # Business logic
│   │   ├── testutil/        # Test utilities
│   │   └── utils/           # Helper functions
│   ├── migrations/          # SQL migration files
│   │   └── seeds/           # Database seed files
│   ├── logs/                # Application logs
│   ├── .env.example         # Environment template
│   ├── docker-compose.yml   # Production Docker config
│   ├── docker-compose.dev.yml # Development Docker config
│   ├── Dockerfile           # Production Dockerfile
│   ├── Dockerfile.dev       # Development Dockerfile
│   ├── Makefile             # Build automation
│   ├── go.mod               # Go dependencies
│   └── main.go              # Application entry point
│
├── frontend/
│   ├── app/                 # Next.js App Router
│   │   ├── menus/          # Menu management page
│   │   ├── layout.tsx      # Root layout
│   │   └── page.tsx        # Home page
│   ├── components/
│   │   ├── ui/             # Base UI components
│   │   ├── menu/           # Menu components
│   │   ├── sidebar/        # Sidebar components
│   │   └── layout/         # Layout components
│   ├── contexts/           # React Context providers
│   ├── lib/                # API client & utilities
│   ├── types/              # TypeScript definitions
│   ├── public/             # Static assets
│   ├── .env.example        # Environment template
│   ├── docker-compose.yml  # Frontend Docker config
│   ├── Dockerfile          # Production Dockerfile
│   ├── next.config.ts      # Next.js configuration
│   ├── package.json        # npm dependencies
│   ├── tailwind.config.ts  # Tailwind configuration
│   └── tsconfig.json       # TypeScript configuration
│
└── README.md               # This file
```

---

## Environment Variables

### Backend (.env)

```bash
# Server Configuration
PORT=4000
ENV=development
APP_NAME="STK Test API"

# Database Configuration
# For Docker: DB_HOST=postgres, DB_PORT=5432
# For Local: DB_HOST=localhost, DB_PORT=5432 or 6543
DB_DRIVER=postgres
DB_HOST=localhost
DB_PORT=6543
DB_USER=postgres
DB_PASSWORD=postgres
DB_NAME=stk_test
DB_SSL_MODE=disable

# JWT Configuration (for future authentication)
JWT_SECRET=your-super-secret-jwt-key-change-this-in-production
JWT_EXPIRY=15m
JWT_REFRESH_EXPIRY=168h

# CORS Configuration
CORS_ALLOWED_ORIGINS=http://localhost:4000,http://localhost:3000
CORS_ALLOWED_METHODS=GET,POST,PUT,PATCH,DELETE,OPTIONS
CORS_ALLOWED_HEADERS=Content-Type,Authorization

# Logging
LOG_LEVEL=info

# Server Timeouts
READ_TIMEOUT=10s
WRITE_TIMEOUT=10s
IDLE_TIMEOUT=60s
```

### Frontend (.env)

```bash
# API Configuration
NEXT_PUBLIC_API_URL=http://localhost:4000

# Node Environment
NODE_ENV=development
```

---

## Technology Choices & Architecture Decisions

### Why Go (Golang)?

1. **Performance**: Go's compiled nature and efficient garbage collection provide excellent performance
2. **Concurrency**: Built-in goroutines for handling concurrent requests
3. **Strong Typing**: Type safety reduces runtime errors
4. **Fast Compilation**: Quick build times for rapid development
5. **Modern Backend**: Growing ecosystem with excellent web frameworks

### Why Fiber Framework?

1. **Express-like API**: Familiar to developers coming from Node.js
2. **Performance**: Built on top of Fasthttp, one of the fastest HTTP engines
3. **Middleware Support**: Easy integration of CORS, logging, compression
4. **Zero Allocation Router**: Efficient request routing

### Why Next.js?

1. **App Router**: Modern routing with React Server Components
2. **Server-Side Rendering**: Better SEO and initial load performance
3. **TypeScript Integration**: First-class TypeScript support
4. **API Routes**: Can serve as BFF (Backend for Frontend) if needed
5. **Developer Experience**: Hot reload, fast refresh, great tooling

### Why PostgreSQL?

1. **Relational Data**: Perfect for hierarchical tree structures
2. **Foreign Keys**: Native support for cascading deletes
3. **JSON Support**: Flexible data storage when needed
4. **Performance**: Excellent query optimization for recursive queries
5. **ACID Compliance**: Data integrity guarantees

### Why GORM?

1. **Migration Management**: Built-in schema migration
2. **Associations**: Easy handling of parent-child relationships
3. **Soft Delete**: Built-in soft delete functionality
4. **Hooks**: BeforeCreate, AfterUpdate hooks for business logic
5. **Developer Friendly**: Intuitive API with chainable methods

### Why Context API (instead of Redux/Zustand)?

1. **Simplicity**: No external dependencies needed
2. **Built-in**: Part of React core
3. **Sufficient**: State complexity doesn't justify Redux
4. **Performance**: Good enough for this use case
5. **Learning Curve**: Lower barrier to entry

### Why react-dnd?

1. **Flexible**: Supports complex drag-and-drop scenarios
2. **HTML5 Backend**: Native browser drag-and-drop API
3. **Customizable**: Full control over drag preview and drop zones
4. **Tree Support**: Works well with nested structures
5. **Active Maintenance**: Regular updates and community support

### Architecture Patterns Used

#### Backend: Clean Architecture
- **Separation of Concerns**: Handlers → Services → Database
- **Dependency Injection**: Services injected into handlers
- **DTO Pattern**: Request validation separated from models
- **Repository Pattern**: Database abstraction through GORM

#### Frontend: Component-Based Architecture
- **Atomic Design**: UI components (atoms → molecules → organisms)
- **Smart/Dumb Components**: Container components vs presentational
- **Context for State**: Global state management
- **Custom Hooks**: Reusable logic extraction

#### Database: Hierarchical Tree Structure
- **Adjacency List**: Simple parent-child references
- **Cascading Deletes**: Automatic cleanup of orphaned nodes
- **Indexing Strategy**: Optimized queries for tree operations

---

## Deployment Recommendations

### Production Checklist

- [ ] Change `JWT_SECRET` to a strong random value
- [ ] Use environment-specific `.env` files
- [ ] Enable SSL/TLS for database connections (`DB_SSL_MODE=require`)
- [ ] Set up database backups
- [ ] Configure reverse proxy (Nginx/Caddy) for HTTPS
- [ ] Enable rate limiting
- [ ] Set up monitoring and logging (e.g., Prometheus, Grafana)
- [ ] Use Docker secrets instead of `.env` files
- [ ] Configure CORS for production domains only
- [ ] Set `ENV=production` in backend

### Scaling Considerations

1. **Database**: Use connection pooling, read replicas for heavy loads
2. **Backend**: Horizontal scaling with load balancer
3. **Frontend**: Deploy to CDN (Vercel, Netlify)
4. **Caching**: Add Redis for session/cache management
5. **Message Queue**: Consider RabbitMQ/Kafka for async operations

---

## Troubleshooting

### Common Issues

#### Database Connection Failed

```bash
# Check PostgreSQL is running
docker ps | grep postgres

# Check connection details in .env
cat backend/.env | grep DB_

# For Docker: DB_HOST=postgres
# For Local: DB_HOST=localhost
```

#### Port Already in Use

```bash
# Find process using port 4000
lsof -i :4000
netstat -ano | findstr :4000  # Windows

# Kill the process
kill -9 <PID>
```

#### Frontend Can't Connect to Backend

```bash
# Check NEXT_PUBLIC_API_URL
cat frontend/.env

# Check CORS settings in backend
cat backend/.env | grep CORS
```

#### Migration Errors

```bash
# Reset database
make docker-reset
make docker-up

# Run migrations again
docker exec -it stk_test_api_app ./app -migrate=sql
```

---

## Acknowledgments

- [Fiber Framework](https://gofiber.io/) - Express-inspired web framework for Go
- [Next.js](https://nextjs.org/) - The React Framework for Production
- [GORM](https://gorm.io/) - Fantastic ORM library for Golang
- [Tailwind CSS](https://tailwindcss.com/) - A utility-first CSS framework
- [react-dnd](https://react-dnd.github.io/react-dnd/) - Drag and Drop for React