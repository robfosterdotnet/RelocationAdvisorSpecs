# Technical Guidance - Sprint 8

**From:** Sarah Chen (Tech Lead)
**Subject:** Integration Testing & Polish Design Specifications

---

## Overview

This document provides design specifications for testing and polishing the application. You'll need to research testing frameworks and write the tests yourself.

---

## Backend Testing with Pytest

### Why Testing Matters

Tests ensure:
- Code works as expected
- Changes don't break existing functionality
- Edge cases are handled
- Documentation of expected behavior

### Project Structure

```
backend/tests/
├── __init__.py
├── conftest.py          # Shared fixtures
├── test_scoring.py      # Scoring service tests
├── test_locations.py    # Location API tests
├── test_rankings.py     # Rankings API tests
└── test_etl.py          # ETL function tests
```

### conftest.py Requirements

Create shared pytest fixtures:

#### Database Session Fixture
- Create a test database (or use SQLite in-memory)
- Create all tables
- Yield a session
- Cleanup after tests

#### Sample Data Fixtures
Create fixtures that provide sample data:
- Sample location
- Sample demographics
- Sample employment
- Sample climate

Research: pytest fixtures, `@pytest.fixture` decorator, scope options

---

### test_scoring.py Requirements

#### TestNormalization Class

Test the normalize function with these cases:

| Test Name | Input | Expected Output |
|-----------|-------|-----------------|
| test_normalize_basic | value=50, min=0, max=100 | 50.0 |
| test_normalize_at_min | value=0, min=0, max=100 | 0.0 |
| test_normalize_at_max | value=100, min=0, max=100 | 100.0 |
| test_normalize_invert | value=25, min=0, max=100, invert=True | 75.0 |
| test_normalize_none | value=None | None |
| test_normalize_equal_min_max | value=50, min=50, max=50 | 50.0 (no division by zero) |

#### TestScoring Class

Test category score calculations:

**test_affordability_score:**
- Create mock demographics with known values
- Calculate score
- Verify it's in expected range (0-100)

**test_job_score:**
- Create mock employment data
- Verify score calculation

**test_climate_score_warm:**
- Test with warm preference
- Higher temp should score higher

**test_climate_score_moderate:**
- Test with moderate preference
- 65°F should score highest

**test_composite_score:**
- Provide known scores and weights
- Verify weighted average is correct

---

### test_locations.py Requirements

Test API endpoints using FastAPI's TestClient.

#### Setup

Research: `fastapi.testclient.TestClient`

#### Tests to Write

| Test Name | Endpoint | Verification |
|-----------|----------|--------------|
| test_list_locations | GET /api/v1/locations | Returns paginated response |
| test_list_locations_pagination | GET /api/v1/locations?page=2 | Returns correct page |
| test_list_locations_filter | GET /api/v1/locations?state=California | Filtered results |
| test_get_location | GET /api/v1/locations/1 | Returns location detail |
| test_get_location_not_found | GET /api/v1/locations/99999 | Returns 404 |
| test_search_locations | GET /api/v1/locations/search?q=los | Returns matching results |

---

### test_rankings.py Requirements

#### Tests to Write

| Test Name | Verification |
|-----------|--------------|
| test_rankings_default_weights | Returns ranked results |
| test_rankings_custom_weights | Weights affect ranking order |
| test_rankings_climate_preference | Climate pref affects scores |
| test_rankings_state_filter | Only returns filtered state |
| test_rankings_invalid_weights | Returns validation error |

---

### Running Tests

**Commands to document:**
```
pytest tests/                    # Run all tests
pytest tests/ -v                 # Verbose output
pytest tests/test_scoring.py    # Run specific file
pytest tests/ -k "normalize"    # Run tests matching pattern
pytest tests/ --cov=app         # Run with coverage
pytest tests/ --cov=app --cov-report=html  # HTML coverage report
```

Research: pytest CLI options, pytest-cov plugin

---

## Frontend Testing

### Setup Requirements

Install testing libraries:
- `@testing-library/react` - React testing utilities
- `@testing-library/jest-dom` - DOM matchers
- `vitest` - Test runner for Vite projects

Research: Vitest configuration, React Testing Library docs

### Test File Structure

```
frontend/src/
├── components/
│   └── common/
│       ├── LocationCard.tsx
│       └── LocationCard.test.tsx  # Co-located tests
└── tests/
    └── setup.ts                   # Test setup file
```

---

### Component Tests to Write

#### LocationCard.test.tsx

| Test Name | Verification |
|-----------|--------------|
| renders location name | Name text visible |
| renders state name | State text visible |
| calls onClick when clicked | Click handler called |
| displays population | Population formatted correctly |
| handles missing data | No crash with null values |

#### Button.test.tsx

| Test Name | Verification |
|-----------|--------------|
| renders children | Button text visible |
| handles click | onClick called |
| disabled state | Button disabled when prop set |
| loading state | Shows spinner when loading |

### Testing Patterns

**Rendering:**
```
render(<Component props={...} />);
```

**Finding Elements:**
- `screen.getByText()` - Find by text content
- `screen.getByRole()` - Find by accessibility role
- `screen.getByTestId()` - Find by data-testid attribute

**Assertions:**
- `expect(element).toBeInTheDocument()`
- `expect(element).toHaveTextContent()`
- `expect(mockFn).toHaveBeenCalled()`

**Interactions:**
- `fireEvent.click(element)`
- `fireEvent.change(input, { target: { value: 'text' } })`

Research: React Testing Library queries, Vitest assertions

---

## Manual Testing Checklist

Create a testing checklist document.

### Home Page
- [ ] Page loads without console errors
- [ ] All navigation links work
- [ ] Call-to-action buttons navigate correctly
- [ ] Responsive on mobile/tablet/desktop

### Explore Page
- [ ] Locations load on page load
- [ ] Loading spinner shows while fetching
- [ ] Pagination Previous/Next work
- [ ] State filter filters results
- [ ] Page resets to 1 when filter changes
- [ ] Clicking card navigates to detail
- [ ] Empty state shows message

### Location Detail Page
- [ ] All data sections display
- [ ] Handles missing data gracefully
- [ ] Back button works
- [ ] Charts render (if applicable)
- [ ] Mobile layout works

### Rankings Page
- [ ] Sliders are draggable
- [ ] Slider values update display
- [ ] Weight sum indicator works
- [ ] Climate preference dropdown works
- [ ] Submit button fetches results
- [ ] Results display ranked correctly
- [ ] Clicking result navigates to detail

### Compare Page
- [ ] Search finds locations
- [ ] Can add locations to compare
- [ ] Remove button works
- [ ] 4-location limit enforced
- [ ] Comparison displays correctly
- [ ] Radar chart renders (if applicable)

### Cross-Browser
- [ ] Chrome works
- [ ] Firefox works
- [ ] Safari works (if applicable)

### Mobile
- [ ] Touch interactions work
- [ ] No horizontal scroll
- [ ] Text readable
- [ ] Buttons tappable (48px minimum)

---

## Performance Optimization

### Backend Optimizations

#### Database
- Verify indexes exist on frequently queried columns
- Use EXPLAIN ANALYZE to check query plans
- Consider caching for expensive queries

#### API
- Use database connection pooling
- Return only needed fields (select specific columns)
- Paginate large results

Research: PostgreSQL EXPLAIN, SQLAlchemy query optimization

### Frontend Optimizations

#### Code Splitting
- Lazy load route components
- Use React.lazy() for large components
- Wrap in Suspense with fallback

#### Performance Patterns
- Use React.memo() for expensive components
- Avoid unnecessary re-renders
- Debounce search inputs

Research: React.lazy, React.memo, useMemo, useCallback

### Measuring Performance

**Frontend:**
- Browser DevTools Performance tab
- Lighthouse audit
- Network tab for API timing

**Backend:**
- Add timing logs to endpoints
- Monitor database query times
- Use profiling tools

---

## Bug Fixing Process

### When You Find a Bug

1. **Reproduce** - Document exact steps to reproduce
2. **Isolate** - Find the specific code causing the issue
3. **Fix** - Make minimal change to fix
4. **Test** - Verify fix works
5. **Regression** - Ensure fix doesn't break other things

### Common Bug Categories

| Category | Symptoms | Common Causes |
|----------|----------|---------------|
| Data | Wrong values displayed | Transform errors, type mismatches |
| UI | Layout broken | CSS issues, missing responsive styles |
| API | 500 errors | Unhandled exceptions, database errors |
| State | Stale data | Missing dependencies in useEffect |
| Type | Runtime errors | Null/undefined not handled |

---

## Research Topics

1. **pytest** - Fixtures, parametrize, markers
2. **pytest-cov** - Code coverage
3. **React Testing Library** - Queries, events, async
4. **Vitest** - Configuration, mocking
5. **Performance profiling** - DevTools, Lighthouse
6. **React optimization** - Memoization, lazy loading

---

## Definition of "Done" for Code Review

When I review your work, I'll check:

1. Are critical paths covered by tests?
2. Do tests actually test the right things?
3. Is the manual checklist complete?
4. Are there any console errors?
5. Does the app work on mobile?
6. Are obvious bugs fixed?

---

*Next: Read `05-developer-tasks.md`*
