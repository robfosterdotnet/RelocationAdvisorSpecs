# Technical Guidance - Sprint 5

**From:** Sarah Chen (Tech Lead)
**Subject:** Scoring Engine Design Specifications

---

## Overview

This document provides design specifications for the scoring and ranking system. You'll need to research the algorithms and write the code yourself.

---

## Core Concepts

### What is Normalization?

To compare different metrics (income in dollars, temperature in degrees, percentages), you need to normalize them to a common scale (0-100).

**Min-Max Normalization Formula:**
```
normalized = (value - min) / (max - min) * 100
```

This converts any value to a 0-100 scale where:
- The lowest value in the dataset becomes 0
- The highest value becomes 100
- All other values fall proportionally between

### Inverted Scoring

Some metrics are "better" when lower:
- Lower rent = more affordable
- Lower unemployment = better job market

For these, invert the normalized score: `100 - normalized_score`

### Weighted Averages

Users assign importance weights to each category. The final score is:
```
composite = (score1 * weight1 + score2 * weight2 + ...) / (weight1 + weight2 + ...)
```

---

## Project Structure

```
backend/app/services/
└── scoring.py          # Scoring service class

backend/app/routers/
└── rankings.py         # Rankings API endpoints

backend/app/schemas/
└── ranking.py          # Request/response schemas
```

---

## Data Classes

### ScoreResult

Create a data class (or Pydantic model) to hold scoring results:

| Field | Type | Description |
|-------|------|-------------|
| location_id | int | Database ID |
| fips_code | str | 5-digit FIPS |
| name | str | County name |
| state | str | State name |
| affordability_score | float | 0-100 score |
| job_score | float | 0-100 score |
| climate_score | float | 0-100 score |
| education_score | float | 0-100 score |
| composite_score | float | Weighted average |

Research: Python `dataclass` decorator or Pydantic `BaseModel`

---

## ScoringService Class Design

### Constructor

```
__init__(self, db: Session)
```
- Store database session
- Initialize empty cache dictionary for min/max values

### Caching Min/Max Values

For each metric, you need the minimum and maximum values across all locations to normalize. These are expensive queries, so cache them.

#### `get_min_max(self, model, column_name)`
- Check if result is in cache
- If not, query `SELECT MIN(col), MAX(col)` from table
- Store in cache
- Return (min_value, max_value) tuple

Research: SQLAlchemy `func.min()` and `func.max()`

---

### Normalization Method

#### `normalize(self, value, min_val, max_val, invert=False)`
- Input: Value to normalize, min/max range, whether to invert
- Output: Normalized score (0-100) or None
- Must handle:
  - None values → return None
  - min equals max → return 50 (avoid division by zero)
  - Standard normalization calculation
  - Inversion if requested
  - Round to 2 decimal places

---

### Category Score Methods

Create separate methods for each category score:

#### `calculate_affordability_score(self, demographics)`
Combines multiple factors:
- Median home value (lower = better, so invert)
- Median gross rent (lower = better, so invert)
- Median income (higher = better buying power)

Average the component scores.

#### `calculate_job_score(self, employment)`
Combines:
- Unemployment rate (lower = better, so invert)
- Median hourly wage (higher = better)

#### `calculate_climate_score(self, climate, preference)`
Based on user preference:
- **"warm"**: Higher temps score higher
- **"cold"**: Lower temps score higher
- **"moderate"**: Closer to 65°F scores higher

For moderate preference, calculate distance from target temperature.

#### `calculate_education_score(self, demographics)`
Based on bachelor's degree percentage.
Higher percentage = higher score.

---

### Composite Score Method

#### `calculate_composite_score(self, scores_dict, weights_dict)`
- Input: Dictionary of category scores, dictionary of weights
- Output: Weighted average score
- Must handle:
  - Missing scores (skip categories with None)
  - Adjust weight sum for missing categories
  - Return 0 if no valid scores

---

### Main Ranking Method

#### `get_rankings(self, weights, climate_preference, state_filter, limit)`

**Parameters:**
| Name | Type | Description |
|------|------|-------------|
| weights | dict | Category weights (must sum to 1.0) |
| climate_preference | str | "warm", "cold", or "moderate" |
| state_filter | str | Optional state name filter |
| limit | int | Max results to return |

**Logic:**
1. Query locations with joined demographics, employment, and climate
2. Apply state filter if provided
3. For each location:
   - Calculate all four category scores
   - Skip locations missing any score
   - Calculate composite score
   - Create ScoreResult object
4. Sort by composite score (descending)
5. Return top N results

---

## Rankings API Design

### schemas/ranking.py Requirements

#### `RankingRequest` Schema
Request body for rankings endpoint:

| Field | Type | Default | Validation |
|-------|------|---------|------------|
| weights | dict | Equal weights (0.25 each) | Must sum to 1.0 |
| climate_preference | str | "moderate" | Must be warm/cold/moderate |
| state_filter | str | None | Optional |
| limit | int | 20 | 1-100 |

**Validation:**
- Create a custom validator that checks weights sum to 1.0 (with small tolerance like 0.99-1.01)
- Create a validator for climate_preference values

Research: Pydantic `@validator` decorator

#### `RankingResponse` Schema
Response containing ScoreResult fields plus any additional computed fields.

---

### routers/rankings.py Requirements

#### Endpoint: `POST /api/v1/rankings`

**Request Body:** RankingRequest

**Response:** List of ranking results

**Logic:**
1. Create ScoringService with database session
2. Call get_rankings with request parameters
3. Convert results to response format
4. Return list

**Why POST not GET?**
The request has a complex body (nested weights object). POST is more appropriate for complex queries even though it's technically a read operation.

---

## Edge Cases to Handle

### Missing Data
- Some locations lack demographics/employment/climate
- Your scoring should skip these gracefully
- Log how many locations were skipped

### Extreme Values
- Verify min/max calculations work correctly
- Check for outliers that might skew scores

### Weight Validation
- Weights must sum to 1.0
- All weights must be non-negative
- Empty weights dictionary should use defaults

### Empty Results
- Handle case where no locations match criteria
- Handle state filter that matches no locations

---

## Testing Strategy

### Unit Tests for Normalization
1. Test normal case: value in middle of range
2. Test edge cases: value equals min, equals max
3. Test inversion: verify 0 becomes 100
4. Test null handling: None input returns None
5. Test division by zero: min equals max

### Unit Tests for Scoring
1. Test each category score calculation
2. Test with realistic data values
3. Test with missing data (None values)

### Integration Tests
1. Test full ranking flow with seeded data
2. Verify sorting is correct
3. Verify state filter works
4. Verify limit is respected

### Manual Verification
1. Pick 3-4 known locations
2. Calculate scores by hand
3. Verify your code produces same results

---

## Research Topics

1. **Min-Max Normalization** - Data science concept for scaling
2. **Weighted Averages** - How to combine multiple scores
3. **Python dataclasses** - Clean data structures
4. **Pydantic validators** - Custom validation logic
5. **SQLAlchemy aggregations** - MIN, MAX, COUNT, AVG functions
6. **API design patterns** - POST vs GET for queries

---

## Definition of "Done" for Code Review

When I review your code, I'll check:

1. Does normalization handle edge cases correctly?
2. Are category scores calculated accurately?
3. Does the weighted average work with missing data?
4. Is the API validated properly?
5. Are results sorted correctly?
6. Can you explain the scoring algorithm verbally?

---

*Next: Read `05-developer-tasks.md`*
