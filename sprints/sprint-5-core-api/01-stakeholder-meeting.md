# Stakeholder Meeting Minutes

**Meeting:** Sprint 5 Planning - API Development
**Attendees:** Marcus Thompson, Sarah Chen, David Park, Andrew

---

## Meeting Notes

### Marcus (Product Owner)

> "We have the data, now let's make it accessible. The API is the contract between our backend and frontend.
>
> **What the frontend needs:**
> 1. List locations with basic info (for browse/search)
> 2. Full details for a single location (for detail page)
> 3. Search by name or state
> 4. Eventually: rankings and comparisons (Sprint 6)
>
> **Performance matters:**
> - Pages should load fast
> - Don't send more data than needed
> - Support pagination (don't load 3,000 counties at once)"

---

### Sarah (Tech Lead)

> "FastAPI makes this straightforward. Here's the approach:
>
> **Project Structure:**
> ```
> app/
> ├── main.py          # FastAPI app, startup
> ├── routers/
> │   ├── locations.py # Location endpoints
> │   └── stats.py     # Statistics endpoints
> └── schemas/
>     ├── location.py  # Response schemas
>     └── common.py    # Shared schemas
> ```
>
> **Key Patterns:**
> - Use Pydantic for all input/output validation
> - Use dependency injection for database sessions
> - Return appropriate HTTP status codes
> - Handle errors gracefully
>
> **Response Format:**
> All responses should follow consistent structure:
> ```json
> {
>   \"data\": [...],
>   \"pagination\": {
>     \"total\": 100,
>     \"page\": 1,
>     \"per_page\": 20
>   }
> }
> ```"

---

### David (Senior Developer)

> "API design tips:
>
> 1. **Be consistent** - Same patterns everywhere
> 2. **Version your API** - Use `/api/v1/` prefix
> 3. **Document everything** - FastAPI does this automatically
> 4. **Handle errors** - Return helpful error messages
> 5. **Think about the frontend** - What data shape do they need?
>
> **Testing:**
> - Use FastAPI's test client
> - Test happy path and error cases
> - Test with real database data"

---

## API Design Decisions

### URL Structure
```
/api/v1/locations           # List
/api/v1/locations/123       # Detail by ID
/api/v1/locations/fips/06037  # Detail by FIPS
/api/v1/locations/search?q=los  # Search
/api/v1/states              # List states
/api/v1/stats               # Database statistics
```

### Response Formats

**List Response:**
```json
{
  "data": [
    {"id": 1, "fips_code": "06037", "name": "Los Angeles County", "state": "California"}
  ],
  "total": 3143,
  "page": 1,
  "per_page": 20
}
```

**Detail Response:**
```json
{
  "id": 1,
  "fips_code": "06037",
  "name": "Los Angeles County",
  "state_name": "California",
  "latitude": 34.05,
  "longitude": -118.24,
  "demographics": {
    "median_household_income": 71358,
    "population": 10014009
  },
  "employment": {
    "unemployment_rate": 4.8
  },
  "climate": {
    "avg_temp_annual": 66.0
  }
}
```

---

## Action Items

| Owner | Action | Due |
|-------|--------|-----|
| Andrew | Create schemas | Day 1-2 |
| Andrew | Implement list/detail endpoints | Day 2-3 |
| Andrew | Add search and filtering | Day 3-4 |
| Andrew | Error handling and testing | Day 4-5 |
| David | Code review | Day 5 |

---

*Next: Read `02-requirements.md`*
