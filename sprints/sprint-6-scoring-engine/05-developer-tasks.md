# Developer Tasks - Sprint 6

---

## Day 1-2: Scoring Service

- [ ] Create `app/services/` directory
- [ ] Create `scoring.py` with ScoringService class
- [ ] Implement min/max caching
- [ ] Implement `normalize()` and `invert_normalize()`
- [ ] Write unit tests for normalization

---

## Day 3: Category Scores

- [ ] Implement `calculate_affordability_score()`
- [ ] Implement `calculate_job_score()`
- [ ] Implement `calculate_climate_score()` with preference
- [ ] Implement `calculate_education_score()`
- [ ] Write unit tests for each calculator

---

## Day 4: API Endpoints

- [ ] Create `routers/rankings.py`
- [ ] Create request/response schemas
- [ ] Implement `POST /api/v1/rankings`
- [ ] Implement `GET /api/v1/compare?ids=`
- [ ] Add validation for weights
- [ ] Test endpoints manually

---

## Day 5: Testing & Polish

- [ ] Complete unit test coverage
- [ ] Test edge cases (NULL data, invalid weights)
- [ ] Integration test with real data
- [ ] Update API documentation
- [ ] Update JOURNAL.md

---

## Verification

```bash
# Test rankings endpoint
curl -X POST http://localhost:8000/api/v1/rankings \
  -H "Content-Type: application/json" \
  -d '{
    "weights": {"affordability": 0.4, "jobs": 0.3, "climate": 0.2, "education": 0.1},
    "climate_preference": "warm",
    "limit": 10
  }'
```

---

*After completion, proceed to `../sprint-7-frontend-foundation/`*
