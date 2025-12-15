# Requirements Document - Sprint 4

---

## API Endpoints

### FR-4.1: List Locations
```
GET /api/v1/locations
```
- Pagination support (page, per_page)
- Filter by state
- Sort by name, population, income
- Returns basic location info (not full details)

### FR-4.2: Get Location Detail
```
GET /api/v1/locations/{id}
GET /api/v1/locations/fips/{fips_code}
```
- Returns full location with all related data
- Includes demographics, employment, climate
- 404 if not found

### FR-4.3: Search Locations
```
GET /api/v1/locations/search?q=query
```
- Search by name (partial match)
- Search by state name
- Returns paginated results

### FR-4.4: List States
```
GET /api/v1/states
```
- Returns list of states with county counts
- Used for filters/dropdowns

### FR-4.5: Statistics
```
GET /api/v1/stats
```
- Total locations, demographics, etc.
- Used for admin/status display

---

## Response Schemas

### LocationBasic
```python
class LocationBasic(BaseModel):
    id: int
    fips_code: str
    name: str
    state_name: str
    population: Optional[int]
```

### LocationDetail
```python
class LocationDetail(BaseModel):
    id: int
    fips_code: str
    name: str
    state_name: str
    latitude: Optional[float]
    longitude: Optional[float]
    demographics: Optional[DemographicsSchema]
    employment: Optional[EmploymentSchema]
    climate: Optional[ClimateSchema]
```

### PaginatedResponse
```python
class PaginatedResponse(BaseModel):
    data: List[Any]
    total: int
    page: int
    per_page: int
```

---

## Non-Functional Requirements

- **Performance:** < 200ms response time
- **Pagination:** Default 20 items, max 100
- **CORS:** Enable for frontend access
- **Documentation:** Auto-generated OpenAPI

---

*Next: Read `03-user-stories.md`*
