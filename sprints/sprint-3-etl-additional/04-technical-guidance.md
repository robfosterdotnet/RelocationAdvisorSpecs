# Technical Guidance - Sprint 3

**From:** Sarah Chen (Tech Lead)
**Subject:** BLS and Climate ETL Design Specifications

---

## Overview

This document provides design specifications for the BLS (Bureau of Labor Statistics) and Climate data ETL pipelines. You'll need to research the APIs and write the code yourself.

---

## BLS API Fundamentals

### Getting Your API Key

1. Visit: https://www.bls.gov/developers/home.htm
2. Complete registration
3. Store in `.env` as `BLS_API_KEY`

### API Structure

**Endpoint:**
```
POST https://api.bls.gov/publicAPI/v2/timeseries/data/
```

**Request Format:** JSON with these fields:
- `seriesid` - Array of series IDs to fetch
- `startyear` - Starting year
- `endyear` - Ending year
- `registrationkey` - Your API key

**Response Format:** Research the BLS API documentation to understand the response structure.

### Series ID Format (Unemployment)

Research how BLS series IDs are constructed. For county unemployment data:
```
LAUCN + {5-digit FIPS} + 0000000003
```

Example: Los Angeles County (FIPS 06037) → `LAUCN060370000000003`

### Rate Limits

- **Without key:** 25 queries/day, 10 series per query
- **With key:** 500 queries/day, 50 series per query

---

## BLS ETL Module Design

### Directory Layout
```
backend/etl/bls/
├── __init__.py
├── client.py       # API client class
├── transform.py    # Data transformation
└── load.py         # Database loading
```

Plus: `backend/etl/run_bls.py` (main entry point)

---

### client.py Requirements

Create a `BLSClient` class that:

1. **Initializes** with API key
2. **Constructs series IDs** from FIPS codes
3. **Batches requests** (max 50 series per request)
4. **Provides `fetch_unemployment(fips_codes, year)`** method that:
   - Builds series IDs for given FIPS codes
   - Splits into batches of 50
   - Makes POST requests with JSON body
   - Handles rate limiting
   - Returns parsed response data
5. **Implements retry logic** for transient failures
6. **Logs all requests**

### Helper Function Needed

Create a `chunks(iterable, size)` function that:
- Takes a list and chunk size
- Yields successive chunks of the specified size
- Used for batching series IDs

---

### transform.py Requirements

Create transformation functions:

#### `parse_bls_response(response_data)`
- Input: Raw API response JSON
- Output: List of dictionaries with unemployment data
- Must:
  - Navigate the BLS response structure (research this!)
  - Extract FIPS code from series ID
  - Get the most recent annual value
  - Handle missing or error data

#### `transform_employment_record(raw_record)`
- Input: Single parsed record from BLS
- Output: Dictionary matching your employment schema
- Must:
  - Map fields to your database columns
  - Handle the FIPS code format (extract from series ID)
  - Convert unemployment rate to float
  - Add data year

---

### load.py Requirements

#### `load_employment(records)`
- Input: List of transformed employment records
- Output: Statistics dictionary
- Must:
  - Look up location by FIPS code
  - Use UPSERT pattern (update if exists, insert if new)
  - Track success/skip/error counts
  - Handle missing locations gracefully

---

## Climate Data Options

### Option A: Download Pre-computed Normals (Recommended)

NOAA provides 30-year climate normals as downloadable CSV files:

1. Visit: https://www.ncei.noaa.gov/products/land-based-station/us-climate-normals
2. Download: 1991-2020 Climate Normals
3. Look for county-level or station-level files

### Option B: NOAA Climate Data API

**Base URL:**
```
https://www.ncdc.noaa.gov/cdo-web/api/v2/
```

**Authentication:** Token in header as `token`

**Key Endpoints:**
- `/data` - Get climate data
- `/stations` - Get station info
- `/locations` - Get location metadata

Research the API documentation at: https://www.ncdc.noaa.gov/cdo-web/webservices/v2

---

## Climate ETL Module Design

### Directory Layout
```
backend/etl/climate/
├── __init__.py
├── fetch.py        # Download/fetch data
├── transform.py    # Data transformation
└── load.py         # Database loading
```

Plus: `backend/etl/run_climate.py`

---

### fetch.py Requirements (Option A - CSV)

#### `download_climate_normals(output_path)`
- Downloads climate normal files from NOAA
- Saves to specified path
- Returns file path(s)

#### `read_climate_csv(filepath)`
- Reads the downloaded CSV file
- Returns list of raw records
- Handle different CSV formats NOAA uses

### fetch.py Requirements (Option B - API)

#### `fetch_climate_data(location_id)`
- Makes API request for climate data
- Handles pagination (API returns limited results)
- Returns raw climate data

---

### transform.py Requirements

#### `transform_climate_record(raw_record)`
- Input: Single record from CSV or API
- Output: Dictionary matching your climate schema
- Must:
  - Map NOAA fields to your database columns
  - Convert temperatures if needed (some files use tenths of degrees)
  - Handle missing data codes

### Station-to-County Mapping Challenge

Weather stations don't directly map to counties. Options:
1. **Use a crosswalk file** - Find FIPS-to-station mapping data
2. **Use coordinates** - Map station lat/lon to county
3. **Average multiple stations** - Some counties have several stations

Research and document your approach!

---

### load.py Requirements

#### `load_climate(records)`
- Same pattern as BLS load
- UPSERT based on location_id
- Track statistics

---

## run_bls.py / run_climate.py Requirements

Each script should:

1. **Parse command-line arguments:**
   - `--dry-run` - Transform without loading
   - `--limit` - Process limited records for testing

2. **Set up logging**

3. **Execute the pipeline:**
   - Fetch data
   - Transform records
   - Load to database (unless dry-run)

4. **Report results**

---

## Data Quality Considerations

### BLS Data Issues
- Not all counties have data
- Some series may be discontinued
- Annual averages vs monthly data

### Climate Data Issues
- Station coverage varies
- Some areas have no nearby stations
- Different time periods for different metrics

### Your Responsibility
- Document which counties have missing data
- Log warnings for incomplete records
- Handle nulls appropriately in database

---

## Testing Strategy

### BLS Testing
1. Start with 5-10 FIPS codes you know have data
2. Verify series ID construction
3. Check parsed values against BLS website

### Climate Testing
1. Start with a single state's data
2. Verify temperature conversions
3. Spot-check against weather websites

---

## Research Topics

1. **BLS API documentation** - Response structure, error codes
2. **NOAA data products** - What files are available, formats
3. **Python CSV module** - Reading/parsing CSV files
4. **Geographic mapping** - How to map stations to counties
5. **Rate limiting strategies** - Sleep, exponential backoff

---

## Definition of "Done" for Code Review

When I review your code, I'll check:

1. Can you fetch BLS data for multiple counties?
2. Does batching work correctly (50 per request)?
3. Can you process climate data for all states?
4. Is station-to-county mapping documented?
5. Are data gaps logged and handled?
6. Is the code idempotent (safe to re-run)?

---

*Next: Read `05-developer-tasks.md`*
