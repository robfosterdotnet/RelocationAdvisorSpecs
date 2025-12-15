# Requirements Document - Sprint 5

---

## Scoring Engine Requirements

### FR-5.1: Data Normalization
- Normalize all metrics to 0-100 scale
- Handle "higher is better" and "lower is better" metrics
- Cache min/max values for performance
- Handle NULL values gracefully

### FR-5.2: Category Scores
Calculate scores for:
- **Affordability Score** (0-100)
- **Job Market Score** (0-100)
- **Climate Score** (0-100, preference-aware)
- **Education Score** (0-100)

### FR-5.3: Weighted Composite Score
- Accept user-defined weights for each category
- Weights must sum to 1.0
- Calculate weighted average of category scores

---

## API Requirements

### FR-5.4: Rankings Endpoint
```
POST /api/v1/rankings
{
  "weights": {
    "affordability": 0.25,
    "jobs": 0.25,
    "climate": 0.25,
    "education": 0.25
  },
  "climate_preference": "moderate",
  "state_filter": null,
  "limit": 20
}
```

Response: Top N locations with scores

### FR-5.5: Comparison Endpoint
```
GET /api/v1/compare?ids=1,2,3
```

Response: Side-by-side data and scores for specified locations

---

## Scoring Formulas

### Normalization
```python
# Higher is better
def normalize(value, min_val, max_val):
    return (value - min_val) / (max_val - min_val) * 100

# Lower is better
def invert_normalize(value, min_val, max_val):
    return (1 - (value - min_val) / (max_val - min_val)) * 100
```

### Category Weights
| Category | Default Weight |
|----------|---------------|
| Affordability | 0.25 |
| Jobs | 0.25 |
| Climate | 0.25 |
| Education | 0.25 |

---

*Next: Read `03-user-stories.md`*
