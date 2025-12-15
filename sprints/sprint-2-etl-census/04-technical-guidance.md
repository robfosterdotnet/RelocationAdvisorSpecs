# Technical Guidance - Sprint 2

**From:** Sarah Chen (Tech Lead)
**Subject:** Census ETL Design Specifications

---

## Overview

This document provides design specifications for the Census data ETL pipeline. You'll need to research the APIs and write the code yourself.

---

## Census API Fundamentals

### Getting Your API Key

1. Visit: https://api.census.gov/data/key_signup.html
2. Complete the registration form
3. Check your email for the key
4. Store it in your `.env` file as `CENSUS_API_KEY`

### API Structure

**Base URL Pattern:**
```
https://api.census.gov/data/{year}/acs/acs5
```

**Query Parameters:**
- `get` - Comma-separated list of variables to retrieve
- `for` - Geographic level (e.g., county:*)
- `in` - Parent geography filter (e.g., state:06)
- `key` - Your API key

### Response Format

The Census API returns a 2D array:
- First row contains column headers
- Subsequent rows contain data
- All values are strings (even numbers!)

### Variables You Need

Research these variable codes at https://api.census.gov/data/2022/acs/acs5/variables.html

| Data Point | Variable Code | Notes |
|------------|---------------|-------|
| Location Name | NAME | County name with state |
| Median Household Income | B19013_001E | In dollars |
| Median Home Value | B25077_001E | In dollars |
| Median Gross Rent | B25064_001E | Monthly, in dollars |
| Total Population | B01003_001E | Count |
| Bachelor's Degree | B15003_022E | Count of people |
| Education Total | B15003_001E | Total for percentage calc |

---

## ETL Module Structure

### Directory Layout
```
backend/etl/
├── __init__.py
├── config.py           # Configuration and constants
├── census/
│   ├── __init__.py
│   ├── client.py       # API client class
│   ├── transform.py    # Data transformation
│   └── load.py         # Database loading
└── run_census.py       # Main entry point
```

---

## Module Design Specifications

### config.py Requirements

This module should define:

1. **CENSUS_API_KEY** - Loaded from environment variable
2. **CENSUS_BASE_URL** - The API base URL
3. **CENSUS_YEAR** - Year of data to fetch (e.g., "2022")
4. **CENSUS_VARIABLES** - List of all variable codes to request
5. **STATE_FIPS** - List of all 51 state/territory FIPS codes (50 states + DC)

**Research:** Find the complete list of state FIPS codes.

---

### client.py Requirements

Create a `CensusClient` class that:

1. **Initializes** with API key (from config or parameter)
2. **Validates** that API key exists
3. **Provides `fetch_county_data(state_fips, variables)`** method that:
   - Constructs the correct API URL
   - Makes the HTTP request
   - Handles HTTP errors
   - Implements retry logic (3 attempts with backoff)
   - Logs requests and responses
   - Returns the parsed JSON response
4. **Provides `close()`** method to clean up resources

### HTTP Client Considerations

- Use the `requests` library
- Consider using a Session for connection reuse
- Add appropriate timeouts
- Log requests for debugging

---

### transform.py Requirements

Create transformation functions:

#### `parse_census_value(value)`
- Input: String value from Census API
- Output: Integer or None
- Must handle:
  - Normal numeric strings ("12345")
  - Census "not available" codes ("-666666666", "-222222222")
  - Empty strings and None values
  - Invalid data

#### `transform_county_record(row, headers)`
- Input: Single data row and header row from Census
- Output: Dictionary matching your demographics schema, or None if invalid
- Must:
  - Create mapping from headers to values
  - Build 5-digit FIPS code from state + county
  - Parse all numeric fields
  - Calculate `bachelors_degree_pct` from count and total
  - Validate data ranges
  - Return None for invalid records

#### `transform_state_data(raw_data)`
- Input: Full API response (list of lists)
- Output: List of transformed record dictionaries
- Must:
  - Extract headers from first row
  - Transform each data row
  - Filter out None results
  - Return list of valid records

---

### load.py Requirements

Create loading functions:

#### `load_demographics(records)`
- Input: List of transformed record dictionaries
- Output: Statistics dictionary (success, skipped, errors counts)
- Must:
  - Create database session
  - For each record:
    - Look up location by FIPS code
    - Skip if location not found (and count as skipped)
    - Use UPSERT pattern (update existing or insert new)
  - Commit transaction
  - Handle errors with rollback
  - Return statistics

### UPSERT Pattern

Research how to implement "insert or update" in SQLAlchemy:
- Check if record exists
- If yes, update fields
- If no, insert new record

---

### run_census.py Requirements

Create the main script that:

1. **Parses command-line arguments:**
   - `--state` - Process single state (optional)
   - `--dry-run` - Transform without loading (optional)

2. **Sets up logging:**
   - Console output with progress
   - File logging with details
   - Timestamps in log messages

3. **Executes the pipeline:**
   - Loop through states (all or specified)
   - For each state: fetch, transform, load
   - Track cumulative statistics
   - Handle errors without stopping entire job

4. **Reports results:**
   - Total records processed
   - Success/skip/error counts
   - Duration

### Command-Line Arguments

Research the `argparse` library for handling CLI arguments.

---

## Important: Populating Locations First

**Problem:** You can't load demographics without locations existing first.

**Solution:** Create an additional script to fetch and load locations:

### Location Data from Census

The Census API can provide location information:
```
https://api.census.gov/data/2022/acs/acs5?get=NAME,GEO_ID&for=county:*&in=state:*
```

You'll need to:
1. Fetch all counties with NAME and GEO_ID
2. Parse county name and state from NAME field
3. Extract FIPS code from GEO_ID
4. Optionally get coordinates (research geocoding or find a dataset)
5. Load into locations table

### Alternative: Manual Seed First

You could also:
1. Start with your seed data from Sprint 1
2. Expand it to cover more counties
3. Then run the demographics ETL

---

## Data Quality Handling

### Census Special Values

| Value | Meaning | Your Action |
|-------|---------|-------------|
| -666666666 | Not available | Set to NULL |
| -222222222 | Too few samples | Set to NULL |
| -333333333 | Median in open interval | Keep the value |
| Empty/"" | No data | Set to NULL |

### Validation Ranges

Before accepting data, validate:
- Income: 0 - 500,000 (flag outliers)
- Home values: 0 - 5,000,000
- Population: 0 - 50,000,000
- Percentages: 0 - 100

---

## Error Handling Design

### Levels of Error Handling

1. **API Level:** Retry transient failures, fail on persistent errors
2. **Record Level:** Skip bad records, continue processing
3. **Batch Level:** Rollback on database errors, report failures
4. **Job Level:** Complete even if some states fail, report summary

### Logging Strategy

Log these events:
- INFO: Starting state, completed state, summary
- WARNING: Skipped records, validation failures
- ERROR: API failures, database errors
- DEBUG: Individual record details (only when debugging)

---

## Testing Strategy

### Test With Small Data First

1. Start with Wyoming (FIPS 56) - only 23 counties
2. Verify data transforms correctly
3. Verify data loads correctly
4. Spot-check against Census website

### Verify Idempotency

Run the script twice with the same state. The second run should:
- Update existing records (not create duplicates)
- Show same count in database

### Spot-Check Data

Pick 5 random counties and manually verify:
1. Go to https://data.census.gov
2. Look up the same county
3. Compare values (may differ slightly by year)

---

## Research Topics

1. **Python `requests` library** - HTTP client
2. **Python `argparse`** - Command-line argument parsing
3. **Python `logging`** - Structured logging
4. **Census API documentation** - Variable codes and geography
5. **SQLAlchemy session management** - Transactions and rollback

---

## Definition of "Done" for Code Review

When I review your code, I'll check:

1. Can you fetch data from Census API successfully?
2. Does transformation handle all edge cases?
3. Is the UPSERT pattern implemented correctly?
4. Does the script run for all states without crashing?
5. Is logging comprehensive and useful?
6. Is the script idempotent (safe to re-run)?
7. Can you explain how you handled data quality issues?

---

*Next: Read `05-developer-tasks.md`*
