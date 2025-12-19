# Technical Guidance - Sprint 4

**From:** Sarah Chen (Tech Lead)
**Subject:** FastAPI Core API Design Specifications

---

## Overview

This document provides design specifications for the REST API. You'll need to research FastAPI patterns and write the code yourself.

---

## Project Structure

```
backend/app/
├── main.py              # App creation, startup, middleware
├── config.py            # Settings management
├── database.py          # DB connection (from Sprint 1)
├── models/              # SQLAlchemy models (from Sprint 1)
├── schemas/             # Pydantic schemas for API
│   ├── __init__.py
│   ├── common.py        # Shared schemas (pagination, errors)
│   ├── location.py      # Location-related schemas
│   └── data.py          # Demographics, employment, climate schemas
├── routers/             # API route handlers
│   ├── __init__.py
│   ├── locations.py     # Location endpoints
│   └── stats.py         # Statistics endpoints
└── services/            # Business logic layer
    └── location_service.py
```

---

## Pydantic Schema Design

### What Are Schemas?

Pydantic schemas define:
- Request body structure (what clients send)
- Response body structure (what API returns)
- Data validation rules
- Type conversions

Research: https://docs.pydantic.dev/

### schemas/common.py Requirements

Create these shared schemas:

#### `PaginatedResponse`
A generic paginated response wrapper containing:
- `data` - List of items (generic type)
- `total` - Total count of all items
- `page` - Current page number
- `per_page` - Items per page
- `pages` - Total number of pages

Research: Pydantic `Generic` types for reusable schemas.

#### `ErrorResponse`
Standard error format containing:
- `error` - Error type/category
- `detail` - Human-readable description

---

### schemas/location.py Requirements

Create these schemas:

#### `LocationBasic`
Summary schema for list views containing:
- `id` - Database ID
- `fips_code` - 5-digit FIPS
- `name` - County name
- `state_name` - State name
- `population` - From demographics (optional)
- `median_income` - From demographics (optional)

#### `LocationDetail`
Full detail schema containing:
- All LocationBasic fields
- `state_fips` - 2-digit state FIPS
- `latitude` - Coordinate (optional)
- `longitude` - Coordinate (optional)
- `demographics` - Nested DemographicsData (optional)
- `employment` - Nested EmploymentData (optional)
- `climate` - Nested ClimateData (optional)

### SQLAlchemy Compatibility

Research how to make Pydantic schemas work with SQLAlchemy models:
- `from_attributes = True` in Config class
- `model_validate()` method

---

### schemas/data.py Requirements

Create schemas for each data type:

#### `DemographicsData`
- `median_household_income` - Optional integer
- `median_home_value` - Optional integer
- `median_gross_rent` - Optional integer
- `population` - Optional integer
- `bachelors_degree_pct` - Optional float
- `data_year` - Optional integer

#### `EmploymentData`
- `unemployment_rate` - Optional float
- `median_hourly_wage` - Optional float
- `job_growth_rate` - Optional float
- `labor_force` - Optional integer
- `data_year` - Optional integer

#### `ClimateData`
- All climate table fields as optional

---

## Router Design

### What Are Routers?

FastAPI routers:
- Group related endpoints
- Apply common prefixes and tags
- Can be included in main app

Research: https://fastapi.tiangolo.com/tutorial/bigger-applications/

### routers/locations.py Requirements

Create a router with prefix `/api/v1/locations`

#### Endpoint: `GET /`
List locations with pagination

**Query Parameters:**
| Parameter | Type | Default | Validation |
|-----------|------|---------|------------|
| page | int | 1 | >= 1 |
| per_page | int | 20 | 1-100 |
| state | str | None | Optional filter |
| sort_by | str | "name" | One of: name, population, income |

**Response:** PaginatedResponse[LocationBasic]

**Logic:**
1. Build SQLAlchemy query
2. Apply filters if provided
3. Count total before pagination
4. Apply sorting
5. Apply offset and limit for pagination
6. Return paginated response

#### Endpoint: `GET /{location_id}`
Get single location by database ID

**Path Parameter:** `location_id` (integer)

**Response:** LocationDetail

**Logic:**
1. Query by ID
2. If not found, raise HTTPException with 404
3. Return full location with related data

#### Endpoint: `GET /fips/{fips_code}`
Get location by FIPS code

**Path Parameter:** `fips_code` (string)

**Response:** LocationDetail

**Logic:** Same as above but filter by fips_code

#### Endpoint: `GET /search`
Search locations by name

**Query Parameters:**
| Parameter | Type | Validation |
|-----------|------|------------|
| q | str | Required, min 2 chars |
| page | int | >= 1 |
| per_page | int | 1-100 |

**Response:** PaginatedResponse[LocationBasic]

**Logic:**
1. Use SQL LIKE/ILIKE for case-insensitive search
2. Search both name and state_name
3. Paginate results

---

### routers/stats.py Requirements

Create a router with prefix `/api/v1/stats`

#### Endpoint: `GET /summary`
Get database statistics

**Response:** Object containing:
- `total_locations` - Count of locations
- `locations_with_demographics` - Count with demographic data
- `locations_with_employment` - Count with employment data
- `locations_with_climate` - Count with climate data
- `states_count` - Distinct states

#### Endpoint: `GET /states`
Get list of all states with location counts

**Response:** List of objects with:
- `state_name` - State name
- `state_fips` - State FIPS code
- `location_count` - Number of counties

---

## Main Application Design

### main.py Requirements

Your main.py should:

1. **Create FastAPI app** with:
   - Title: "Relocation Advisor API"
   - Description
   - Version

2. **Configure CORS middleware** for:
   - Allow frontend origin (localhost:3000 for dev)
   - Allow credentials
   - Allow all methods and headers

3. **Include routers**

4. **Add startup event** to:
   - Create database tables if needed
   - Log startup message

5. **Add health check endpoint** at `/health`

Research: FastAPI middleware, CORS configuration

---

## Dependency Injection

### Database Sessions

FastAPI uses dependency injection for database sessions.

Research how `Depends(get_db)` works:
- Creates session per request
- Yields session to route handler
- Cleans up after request completes

### Using Dependencies

Every route that needs database access should:
```
def my_route(..., db: Session = Depends(get_db)):
```

---

## Error Handling

### HTTP Exceptions

For errors, raise `HTTPException`:
- 404 for not found
- 400 for bad request
- 422 for validation errors (automatic)
- 500 for server errors

### Error Response Format

All errors should return JSON with:
- `detail` - Error message

---

## API Documentation

FastAPI auto-generates documentation:
- Swagger UI at `/docs`
- ReDoc at `/redoc`
- OpenAPI JSON at `/openapi.json`

### Improving Documentation

Add docstrings to your route handlers. Research how FastAPI uses docstrings in the generated docs.

---

## Testing Strategy

### Using TestClient

FastAPI provides `TestClient` for testing.

Research: https://fastapi.tiangolo.com/tutorial/testing/

### Tests to Write

Create `tests/test_locations.py` with tests for:

1. `test_list_locations` - Verify pagination response structure
2. `test_get_location` - Verify single location response
3. `test_get_location_not_found` - Verify 404 response
4. `test_search` - Verify search functionality
5. `test_filter_by_state` - Verify state filtering

### Running Tests

```bash
pytest tests/
```

---

## Research Topics

1. **FastAPI tutorial** - https://fastapi.tiangolo.com/tutorial/
2. **Pydantic documentation** - Validation, types, configs
3. **SQLAlchemy queries** - Filtering, joining, counting
4. **HTTP status codes** - When to use 200, 201, 400, 404, etc.
5. **CORS** - What it is, why it's needed
6. **Dependency injection** - FastAPI's Depends system

---

## Definition of "Done" for Code Review

When I review your code, I'll check:

1. Do all endpoints return correct response formats?
2. Is pagination implemented correctly?
3. Are errors handled with appropriate status codes?
4. Does search work case-insensitively?
5. Is the code organized into proper layers?
6. Do the auto-generated docs look good?
7. Do tests pass?

---

*Next: Read `05-developer-tasks.md`*
