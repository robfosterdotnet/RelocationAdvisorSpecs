# Requirements Document - Sprint 2

**Document Version:** 1.0
**Author:** Sarah Chen (Tech Lead)

---

## Overview

This sprint implements the first ETL pipeline to ingest demographic data from the U.S. Census Bureau's American Community Survey (ACS).

---

## Functional Requirements

### FR-2.1: Census API Integration

The system must:
- Authenticate with Census API using an API key
- Fetch data from ACS 5-Year Estimates
- Support fetching data for all 50 states + DC
- Handle API rate limiting gracefully

### FR-2.2: Data Variables

The pipeline must fetch:

| Variable | Census Code | Our Field |
|----------|-------------|-----------|
| Median Household Income | B19013_001E | median_household_income |
| Median Home Value | B25077_001E | median_home_value |
| Median Gross Rent | B25064_001E | median_gross_rent |
| Total Population | B01003_001E | population |
| Bachelor's Degree Count | B15003_022E | (for calculation) |
| Education Total | B15003_001E | (for calculation) |

### FR-2.3: Data Transformation

The pipeline must:
- Convert string values to appropriate numeric types
- Calculate `bachelors_degree_pct` from raw counts
- Handle Census "not available" codes (-666666666)
- Handle NULL and empty values
- Construct FIPS codes from state and county codes

### FR-2.4: Data Loading

The pipeline must:
- Match records to existing locations by FIPS code
- Update demographics table with fetched data
- Set appropriate data_year based on source
- Use UPSERT pattern (insert or update)

### FR-2.5: Error Handling

The pipeline must:
- Log all API requests and responses
- Continue processing if individual records fail
- Report summary statistics (success/fail counts)
- Save errors to a log file for review

---

## Non-Functional Requirements

### NFR-2.1: Performance
- Complete full ETL in under 10 minutes
- Batch database operations where possible

### NFR-2.2: Reliability
- Idempotent: safe to run multiple times
- Resume capability: can restart from failure point

### NFR-2.3: Observability
- Console output showing progress
- Detailed log file
- Summary report at completion

### NFR-2.4: Configuration
- API key from environment variable
- Configurable year and dataset
- Dry-run option for testing

---

## Data Quality Rules

### Valid Values
| Field | Valid Range | Action if Invalid |
|-------|-------------|-------------------|
| median_household_income | 0 - 500,000 | Set to NULL |
| median_home_value | 0 - 5,000,000 | Set to NULL |
| median_gross_rent | 0 - 10,000 | Set to NULL |
| population | 0 - 50,000,000 | Set to NULL |
| bachelors_degree_pct | 0 - 100 | Set to NULL |

### Census Special Codes
| Code | Meaning | Action |
|------|---------|--------|
| -666666666 | Not available | Set to NULL |
| -222222222 | Too few samples | Set to NULL |
| -333333333 | Median falls in open-ended interval | Keep value |
| null/empty | No data | Set to NULL |

---

## Acceptance Criteria

1. Script runs without errors for all states
2. Demographics table populated for 3,000+ counties
3. Data passes validation checks
4. Log file shows processing details
5. Manual spot-check matches Census website
6. Script is idempotent

---

## Out of Scope

- Historical data (multiple years)
- State-level aggregations
- Error retry with exponential backoff (stretch goal)
- Parallel processing (stretch goal)

---

*Next: Read `03-user-stories.md`*
