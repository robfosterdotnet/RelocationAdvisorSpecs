# Definition of Done - Sprint 4

---

## Completion Checklist

### Schemas ✓
- [ ] All Pydantic schemas created
- [ ] Schemas validate correctly
- [ ] from_attributes config for SQLAlchemy

### Endpoints ✓
- [ ] `GET /api/v1/locations` - paginated list
- [ ] `GET /api/v1/locations/{id}` - detail
- [ ] `GET /api/v1/locations/fips/{fips}` - detail by FIPS
- [ ] `GET /api/v1/locations/search?q=` - search
- [ ] `GET /api/v1/states` - state list
- [ ] `GET /api/v1/stats` - statistics

### Features ✓
- [ ] Pagination works (page, per_page)
- [ ] Filtering by state works
- [ ] Sorting works
- [ ] Search works (case-insensitive)

### Quality ✓
- [ ] 404 for missing resources
- [ ] Consistent error format
- [ ] CORS enabled
- [ ] Tests pass

### Documentation ✓
- [ ] `/docs` shows all endpoints
- [ ] Endpoints have descriptions
- [ ] JOURNAL.md updated

---

## Quick Test

```bash
# All should return valid JSON
curl -s http://localhost:8000/api/v1/locations | head
curl -s http://localhost:8000/api/v1/locations/1
curl -s "http://localhost:8000/api/v1/locations/search?q=new"
curl -s http://localhost:8000/api/v1/states
```

---

*Proceed to `../sprint-5-scoring-engine/`*
