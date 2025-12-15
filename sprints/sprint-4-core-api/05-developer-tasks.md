# Developer Tasks - Sprint 4

---

## Day 1-2: Schemas and Setup

### Day 1
- [ ] Create `app/schemas/` directory
- [ ] Create `common.py` with PaginatedResponse, ErrorResponse
- [ ] Create `location.py` with LocationBasic, LocationDetail
- [ ] Create `data.py` with Demographics, Employment, Climate schemas
- [ ] Update `schemas/__init__.py` with exports
- [ ] Test schemas import correctly

### Day 2
- [ ] Create `app/routers/` directory
- [ ] Set up `locations.py` router skeleton
- [ ] Set up `stats.py` router skeleton
- [ ] Update `main.py` to include routers
- [ ] Add CORS middleware
- [ ] Verify `/docs` shows new routes

---

## Day 3-4: Implement Endpoints

### Day 3
- [ ] Implement `GET /api/v1/locations` with pagination
- [ ] Implement `GET /api/v1/locations/{id}`
- [ ] Implement `GET /api/v1/locations/fips/{fips}`
- [ ] Test each endpoint manually

### Day 4
- [ ] Implement `GET /api/v1/locations/search`
- [ ] Implement `GET /api/v1/states`
- [ ] Implement `GET /api/v1/stats`
- [ ] Add filtering and sorting to list endpoint

---

## Day 5: Testing and Polish

### Testing
- [ ] Create `tests/test_locations.py`
- [ ] Write tests for each endpoint
- [ ] Test error cases (404, validation)
- [ ] Run `pytest` - all pass

### Error Handling
- [ ] Add global exception handler
- [ ] Ensure consistent error responses
- [ ] Log errors appropriately

### Documentation
- [ ] Review auto-generated docs at `/docs`
- [ ] Add docstrings to all endpoints
- [ ] Update JOURNAL.md

### Final Commit
```bash
git add .
git commit -m "feat: implement core REST API"
git push
```

---

## Verification

```bash
# Start server
uvicorn app.main:app --reload

# Test endpoints
curl http://localhost:8000/api/v1/locations | jq
curl http://localhost:8000/api/v1/locations/1 | jq
curl "http://localhost:8000/api/v1/locations/search?q=los" | jq

# Run tests
pytest tests/
```

---

*After completion, proceed to `../sprint-5-scoring-engine/`*
