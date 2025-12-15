# User Stories - Sprint 2

**Sprint:** 2 - ETL Pipeline: Census Data
**Assigned To:** Andrew

---

## Story S2-1: Census API Setup

**As a** developer
**I want** to authenticate with the Census API
**So that** I can fetch demographic data

### Acceptance Criteria
- [ ] API key obtained from Census Bureau
- [ ] API key stored in `.env` file (not in code)
- [ ] Can make successful test request to Census API
- [ ] API key is documented in `.env.example` (without actual value)

### Story Points: 2

---

## Story S2-2: ETL Project Structure

**As a** developer
**I want** a well-organized ETL module structure
**So that** data pipelines are maintainable and testable

### Acceptance Criteria
- [ ] `backend/etl/census/` directory created
- [ ] Separate modules for client, fetch, transform, load
- [ ] Configuration module for settings
- [ ] Entry point script `run_census.py`
- [ ] Proper `__init__.py` files

### Story Points: 2

---

## Story S2-3: Census API Client

**As a** developer
**I want** a reusable Census API client class
**So that** I can make API calls consistently

### Acceptance Criteria
- [ ] `CensusClient` class with API key handling
- [ ] Method to fetch data for a state
- [ ] Proper error handling for API failures
- [ ] Logging of requests and responses
- [ ] Configurable base URL and year

### Story Points: 3

---

## Story S2-4: Data Fetching

**As a** developer
**I want** to fetch demographic data for all US counties
**So that** I have raw data to process

### Acceptance Criteria
- [ ] Fetch data for all 50 states + DC
- [ ] Request all required Census variables
- [ ] Handle rate limiting (add delays if needed)
- [ ] Progress indicator during fetch
- [ ] Store raw responses for debugging

### Story Points: 5

---

## Story S2-5: Data Transformation

**As a** developer
**I want** to clean and transform Census data
**So that** it matches our database schema

### Acceptance Criteria
- [ ] Parse JSON responses into structured data
- [ ] Convert strings to appropriate types
- [ ] Handle Census special codes (-666666666, etc.)
- [ ] Calculate `bachelors_degree_pct` from counts
- [ ] Construct 5-digit FIPS code
- [ ] Validate values against reasonable ranges
- [ ] Set data_year field

### Story Points: 5

---

## Story S2-6: Data Loading

**As a** developer
**I want** to load transformed data into the database
**So that** the demographics table is populated

### Acceptance Criteria
- [ ] Match records by FIPS code to locations
- [ ] Skip records without matching location
- [ ] Use UPSERT pattern (update if exists, insert if not)
- [ ] Batch operations for performance
- [ ] Transaction handling (commit or rollback)
- [ ] Report number of records processed

### Story Points: 4

---

## Story S2-7: Logging and Error Handling

**As a** developer
**I want** comprehensive logging
**So that** I can debug issues and monitor progress

### Acceptance Criteria
- [ ] Console output with progress bar or status
- [ ] Log file with detailed information
- [ ] Error records saved separately for review
- [ ] Summary statistics at completion
- [ ] No unhandled exceptions crash the script

### Story Points: 3

---

## Story S2-8: Testing and Verification

**As a** developer
**I want** to verify the ETL pipeline works correctly
**So that** I can trust the data quality

### Acceptance Criteria
- [ ] Tested with single state before full run
- [ ] Full run completes successfully
- [ ] 3,000+ county records in demographics table
- [ ] Spot-check 5 counties against Census website
- [ ] No obvious data quality issues
- [ ] Script is idempotent (run twice, same result)

### Story Points: 3

---

## Sprint Velocity Target

**Total Story Points:** 27

This is a challenging sprint with real external API integration.

---

## Story Dependencies

```
S2-1 (API Setup)
    ↓
S2-2 (Project Structure)
    ↓
S2-3 (API Client)
    ↓
S2-4 (Fetching) → S2-5 (Transform) → S2-6 (Loading)
                                          ↓
                                    S2-7 (Logging)
                                          ↓
                                    S2-8 (Testing)
```

---

*Next: Read `04-technical-guidance.md`*
