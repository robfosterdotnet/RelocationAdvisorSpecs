# User Stories - Sprint 4

---

## Story S4-1: Pydantic Schemas (3 points)
Create all request/response schemas.

### Acceptance Criteria
- [ ] LocationBasic schema
- [ ] LocationDetail schema
- [ ] DemographicsSchema, EmploymentSchema, ClimateSchema
- [ ] PaginatedResponse schema
- [ ] Schemas in `app/schemas/`

---

## Story S4-2: List Locations Endpoint (5 points)
Implement paginated location list.

### Acceptance Criteria
- [ ] `GET /api/v1/locations` works
- [ ] Pagination with page/per_page params
- [ ] Filter by state with `?state=California`
- [ ] Sort options
- [ ] Returns LocationBasic schema

---

## Story S4-3: Location Detail Endpoint (5 points)
Get single location with all data.

### Acceptance Criteria
- [ ] `GET /api/v1/locations/{id}` works
- [ ] `GET /api/v1/locations/fips/{fips}` works
- [ ] Returns LocationDetail with nested objects
- [ ] 404 for not found

---

## Story S4-4: Search Endpoint (4 points)
Search locations by name.

### Acceptance Criteria
- [ ] `GET /api/v1/locations/search?q=text` works
- [ ] Searches name and state
- [ ] Case-insensitive
- [ ] Paginated results

---

## Story S4-5: Supporting Endpoints (3 points)
Build helper endpoints.

### Acceptance Criteria
- [ ] `GET /api/v1/states` lists states
- [ ] `GET /api/v1/stats` returns counts
- [ ] Proper response formats

---

## Story S4-6: Error Handling (3 points)
Consistent error responses.

### Acceptance Criteria
- [ ] 400 for bad requests
- [ ] 404 for not found
- [ ] 500 errors logged
- [ ] Error response schema

---

## Story S4-7: API Testing (3 points)
Test all endpoints.

### Acceptance Criteria
- [ ] Test file in `tests/`
- [ ] Happy path tests
- [ ] Error case tests
- [ ] All tests pass

---

**Total Story Points:** 26

---

*Next: Read `04-technical-guidance.md`*
