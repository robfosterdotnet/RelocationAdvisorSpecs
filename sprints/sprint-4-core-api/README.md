# Sprint 4: Core API Development

**Duration:** 5-6 days
**Sprint Goal:** Build the REST API that exposes location data to the frontend

---

## Sprint Overview

Now that we have data, we need an API to serve it. You'll learn:

- RESTful API design principles
- FastAPI route handlers
- Pydantic schemas for validation
- Database queries with SQLAlchemy
- API documentation with OpenAPI/Swagger

---

## What You're Building

### API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/locations` | List all locations (paginated) |
| GET | `/api/locations/{id}` | Get single location with all data |
| GET | `/api/locations/search` | Search by name/state |
| GET | `/api/states` | List all states |
| GET | `/api/stats` | Database statistics |

---

## Sprint Deliverables

- [ ] All CRUD endpoints implemented
- [ ] Pydantic schemas for request/response
- [ ] Pagination support
- [ ] Search and filtering
- [ ] Error handling
- [ ] API documentation (auto-generated)
- [ ] Journal entry for Sprint 4

---

## Key Concepts

- **REST** - Representational State Transfer
- **CRUD** - Create, Read, Update, Delete
- **Pagination** - Returning data in pages
- **Schema Validation** - Ensuring correct data shapes
- **Dependency Injection** - FastAPI's DI system

---

## Sprint Contents

| Document | Purpose |
|----------|---------|
| `01-stakeholder-meeting.md` | API requirements discussion |
| `02-requirements.md` | API specifications |
| `03-user-stories.md` | Assigned tasks |
| `04-technical-guidance.md` | FastAPI patterns and examples |
| `05-developer-tasks.md` | Implementation checklist |
| `06-learning-resources.md` | REST API and FastAPI learning |
| `07-definition-of-done.md` | Completion criteria |

---

## Start Here

Open `01-stakeholder-meeting.md` and begin reading.
