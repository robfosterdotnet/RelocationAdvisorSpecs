# Technical Guidance - Sprint 10

**From:** Sarah Chen (Tech Lead)
**Subject:** Deployment & Documentation Design Specifications

---

## Overview

This document provides design specifications for containerization, deployment, and documentation. You'll need to research Docker and deployment platforms, then write the configuration files yourself.

---

## Docker Fundamentals

### What is Docker?

Docker packages applications with their dependencies into containers that run consistently across environments.

**Key Concepts:**
- **Image** - Blueprint for a container (like a class)
- **Container** - Running instance of an image (like an object)
- **Dockerfile** - Instructions to build an image
- **docker-compose** - Run multiple containers together

Research: Docker Getting Started guide

---

## Backend Dockerfile Requirements

Create `backend/Dockerfile`

### Build Steps Needed

1. **Base Image:** Use Python 3.11 slim image
2. **Working Directory:** Set to /app
3. **Dependencies:** Copy requirements.txt and install
4. **Application:** Copy all source code
5. **Port:** Expose port 8000
6. **Command:** Run uvicorn with host 0.0.0.0

### Best Practices
- Use slim base images (smaller size)
- Copy requirements.txt separately (better caching)
- Use --no-cache-dir for pip (smaller image)
- Don't run as root in production

Research: Dockerfile reference, Python Docker best practices

---

## Frontend Dockerfile Requirements

Create `frontend/Dockerfile`

### Multi-Stage Build

Use two stages for smaller production image:

**Stage 1: Builder**
1. Use Node 18 alpine as base
2. Copy package files
3. Install dependencies
4. Copy source code
5. Run build command

**Stage 2: Production**
1. Use nginx alpine as base
2. Copy built files from builder stage
3. Copy nginx configuration
4. Expose port 80
5. Run nginx

### Why Multi-Stage?
- Build tools not needed in production
- Final image is much smaller
- Only contains compiled assets

Research: Multi-stage Docker builds, nginx static file serving

---

## Nginx Configuration

Create `frontend/nginx.conf`

### Requirements

Configure nginx to:
- Serve static files from /usr/share/nginx/html
- Handle React Router (redirect all routes to index.html)
- Enable gzip compression
- Set appropriate cache headers

Research: nginx configuration for SPAs

---

## Docker Compose Requirements

Create `docker-compose.yml` at project root

### Services to Define

#### db (PostgreSQL)
- Image: postgres:15
- Environment variables for database name, user, password
- Volume for data persistence
- Port mapping (5432:5432)

#### backend
- Build from ./backend
- Port mapping (8000:8000)
- Environment: DATABASE_URL pointing to db service
- Depends on: db

#### frontend
- Build from ./frontend
- Port mapping (3000:80)
- Environment: VITE_API_URL

### Volumes
- Define named volume for postgres data

### Network
Docker Compose creates a default network where services can reach each other by service name.

Research: Docker Compose file reference, service dependencies

---

## Local Development with Docker

### Commands to Document

```
docker-compose up --build       # Build and start all services
docker-compose up -d            # Start in background
docker-compose logs backend     # View specific service logs
docker-compose down             # Stop all services
docker-compose down -v          # Stop and remove volumes
```

### Environment File

Create `.env` file for sensitive values (not committed to git):
```
POSTGRES_PASSWORD=...
CENSUS_API_KEY=...
BLS_API_KEY=...
```

Reference in docker-compose using `${VARIABLE_NAME}`

---

## Deployment Options

### Option 1: Railway (Recommended for Beginners)

**Pros:**
- Simple GitHub integration
- Managed PostgreSQL
- Free tier available
- Automatic deployments

**Steps:**
1. Create Railway account
2. Connect GitHub repository
3. Add PostgreSQL service
4. Configure environment variables
5. Deploy backend service
6. Deploy frontend service

Research: Railway documentation

### Option 2: Render + Vercel

**Render (Backend + Database):**
- Create Web Service for FastAPI
- Create PostgreSQL database
- Set environment variables

**Vercel (Frontend):**
- Connect GitHub repo
- Configure build settings
- Set VITE_API_URL environment variable

Research: Render FastAPI deployment, Vercel React deployment

### Option 3: Fly.io

**Pros:**
- More control
- Global edge deployment
- Good free tier

**Cons:**
- More complex setup
- Requires CLI tool

Research: Fly.io documentation

---

## Production Environment Variables

### Backend
| Variable | Description | Example |
|----------|-------------|---------|
| DATABASE_URL | Full PostgreSQL connection string | postgresql://user:pass@host:5432/db |
| API_HOST | Host to bind to | 0.0.0.0 |
| API_PORT | Port to listen on | 8000 |
| DEBUG | Debug mode | false |
| CORS_ORIGINS | Allowed frontend URLs | https://yourapp.com |

### Frontend
| Variable | Description | Example |
|----------|-------------|---------|
| VITE_API_URL | Backend API URL | https://api.yourapp.com |

---

## Production Checklist

Before deploying:

### Security
- [ ] Debug mode disabled
- [ ] API keys not in code
- [ ] CORS configured correctly
- [ ] HTTPS enabled (usually automatic on platforms)

### Database
- [ ] Production database created
- [ ] Connection string configured
- [ ] Migrations run
- [ ] Data seeded (or ETL run)

### Testing
- [ ] Application works locally with Docker
- [ ] All tests pass
- [ ] Manual testing complete

---

## Documentation Requirements

### README.md Updates

Your main README should include:

1. **Project Title and Description**
   - One-line summary
   - Key features (3-5 bullets)

2. **Live Demo Link**
   - Link to deployed application
   - Link to API documentation (/docs)

3. **Screenshots**
   - Home page
   - Rankings page with results
   - Location detail

4. **Tech Stack**
   - Backend technologies
   - Frontend technologies
   - Data sources

5. **Quick Start**
   - Prerequisites
   - Clone command
   - Setup steps
   - Run commands

6. **Architecture**
   - High-level diagram
   - Data flow description

7. **API Documentation**
   - Link to auto-generated docs
   - Key endpoints summary

8. **Author**
   - Your name
   - Contact/LinkedIn (optional)

### Screenshots

Take screenshots of:
- Home page (desktop)
- Explore page with results
- Rankings page with sliders
- Location detail page
- Mobile view

Save in `docs/screenshots/` directory

---

## Portfolio Preparation

### JOURNAL.md

Document your learning journey:
- Challenges faced
- Solutions discovered
- Skills learned
- Time invested

### Resume Bullet Points

Prepare 2-3 bullet points:
- "Built full-stack relocation advisor using FastAPI, React, and PostgreSQL"
- "Integrated 3 federal data APIs (Census, BLS, NOAA) via ETL pipelines"
- "Implemented weighted scoring algorithm with interactive visualizations"

### Interview Talking Points

Be ready to discuss:
- **Architecture decisions** - Why FastAPI? Why PostgreSQL?
- **Challenges** - Data quality issues, API rate limits
- **Tradeoffs** - Performance vs complexity
- **What you'd do differently** - Lessons learned

---

## Research Topics

1. **Docker** - Images, containers, volumes
2. **Docker Compose** - Multi-container applications
3. **nginx** - Static file serving, reverse proxy
4. **Railway/Render/Vercel** - Platform-specific deployment
5. **Environment variables** - Secrets management
6. **README best practices** - Documentation standards

---

## Definition of "Done" for Project Completion

When you're done:

1. Application runs locally with Docker
2. Application deployed and accessible
3. All features work in production
4. README is comprehensive
5. Screenshots included
6. API docs accessible
7. JOURNAL.md complete
8. You can explain the project verbally

---

*Next: Read `05-developer-tasks.md`*
