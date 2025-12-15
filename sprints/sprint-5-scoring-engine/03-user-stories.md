# User Stories - Sprint 5

---

## Story S5-1: Scoring Service Setup (3 points)
Create the scoring service module.

### Acceptance Criteria
- [ ] `app/services/scoring.py` created
- [ ] ScoringService class structure
- [ ] Min/max caching mechanism

---

## Story S5-2: Data Normalization (5 points)
Implement normalization functions.

### Acceptance Criteria
- [ ] `normalize()` function works
- [ ] `invert_normalize()` function works
- [ ] Handles NULL values
- [ ] Unit tests pass

---

## Story S5-3: Category Score Calculation (8 points)
Calculate individual category scores.

### Acceptance Criteria
- [ ] `calculate_affordability_score()` works
- [ ] `calculate_job_score()` works
- [ ] `calculate_climate_score()` works (with preference)
- [ ] `calculate_education_score()` works
- [ ] All unit tested

---

## Story S5-4: Rankings Endpoint (5 points)
Build the rankings API.

### Acceptance Criteria
- [ ] `POST /api/v1/rankings` endpoint
- [ ] Accepts weights and preferences
- [ ] Returns sorted results with scores
- [ ] Pagination support

---

## Story S5-5: Compare Endpoint (3 points)
Build comparison API.

### Acceptance Criteria
- [ ] `GET /api/v1/compare?ids=1,2,3`
- [ ] Returns side-by-side scores
- [ ] Handles invalid IDs

---

## Story S5-6: Validation & Edge Cases (3 points)
Handle edge cases properly.

### Acceptance Criteria
- [ ] Weights must sum to ~1.0
- [ ] Invalid preferences rejected
- [ ] Locations with missing data handled

---

**Total Story Points:** 27

---

*Next: Read `04-technical-guidance.md`*
