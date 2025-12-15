# Developer Tasks - Sprint 2

**Sprint:** 2 - ETL Pipeline: Census Data
**Developer:** Andrew

---

## Day 1: Setup and API Exploration

### Task 1.1: Get Census API Key
- [ ] Go to https://api.census.gov/data/key_signup.html
- [ ] Fill out the form
- [ ] Check email for API key
- [ ] Add to `.env`: `CENSUS_API_KEY=your_key_here`
- [ ] Add to `.env.example`: `CENSUS_API_KEY=your_census_api_key_here`

### Task 1.2: Explore the Census API
- [ ] Read API documentation: https://www.census.gov/data/developers/guidance.html
- [ ] Use browser or curl to test a request:
```bash
curl "https://api.census.gov/data/2022/acs/acs5?get=NAME,B19013_001E&for=county:*&in=state:56&key=YOUR_KEY"
```
- [ ] Understand the response format
- [ ] Test with different variables

### Task 1.3: Create ETL Directory Structure
```bash
mkdir -p backend/etl/census
touch backend/etl/__init__.py
touch backend/etl/config.py
touch backend/etl/census/__init__.py
touch backend/etl/census/client.py
touch backend/etl/census/fetch.py
touch backend/etl/census/transform.py
touch backend/etl/census/load.py
touch backend/etl/run_census.py
```
- [ ] All files created
- [ ] Commit: `git commit -m "chore: add ETL directory structure"`

### Task 1.4: Create Configuration Module
Create `backend/etl/config.py`:
- [ ] Load environment variables
- [ ] Define CENSUS_API_KEY
- [ ] Define CENSUS_VARIABLES list
- [ ] Define STATE_FIPS list (all 51 - 50 states + DC)

---

## Day 2: Census API Client

### Task 2.1: Install Required Packages
```bash
cd backend
source venv/bin/activate
pip install requests
pip freeze > requirements.txt
```
- [ ] Requests installed

### Task 2.2: Create CensusClient Class
Create `backend/etl/census/client.py`:
- [ ] `__init__` method with API key
- [ ] `fetch_county_data(state_fips, variables)` method
- [ ] Error handling with retries
- [ ] Logging of requests
- [ ] `close()` method

### Task 2.3: Test the Client
```python
# In Python REPL
from etl.census.client import CensusClient
from etl.config import CENSUS_VARIABLES

client = CensusClient()
data = client.fetch_county_data("56", CENSUS_VARIABLES)  # Wyoming (small)
print(f"Got {len(data)} rows")
print(data[0])  # Headers
print(data[1])  # First county
client.close()
```
- [ ] Client fetches data successfully
- [ ] Commit: `git commit -m "feat: add Census API client"`

---

## Day 3: Data Transformation

### Task 3.1: Create Transform Functions
Create `backend/etl/census/transform.py`:
- [ ] `parse_census_value(value)` - Convert string to int
- [ ] Handle special codes (-666666666, etc.)
- [ ] `transform_county_record(row, headers)` - Single record
- [ ] Build FIPS code from state + county
- [ ] Calculate bachelors_degree_pct
- [ ] `transform_state_data(raw_data)` - All records

### Task 3.2: Test Transformation
```python
from etl.census.client import CensusClient
from etl.census.transform import transform_state_data
from etl.config import CENSUS_VARIABLES

client = CensusClient()
raw = client.fetch_county_data("56", CENSUS_VARIABLES)
records = transform_state_data(raw)

print(f"Transformed {len(records)} records")
for r in records[:3]:
    print(r)
```
- [ ] All fields correctly extracted
- [ ] FIPS codes are 5 digits
- [ ] Percentages calculated correctly
- [ ] Special codes handled (show as None)
- [ ] Commit: `git commit -m "feat: add Census data transformation"`

---

## Day 4: Data Loading

### Task 4.1: Ensure Locations Are Populated
Before loading demographics, we need locations. Either:
- [ ] Run the seed script from Sprint 1
- [ ] OR create a locations ETL first (see note below)

**Note:** You may need to create a simple script to fetch location data (names, coordinates) from Census. This is additional work but necessary for the demographics to have somewhere to go.

### Task 4.2: Create Load Functions
Create `backend/etl/census/load.py`:
- [ ] `load_demographics(records)` function
- [ ] Look up location by FIPS code
- [ ] Skip if location not found
- [ ] UPSERT pattern (update or insert)
- [ ] Return statistics (success/skipped/errors)
- [ ] Proper transaction handling

### Task 4.3: Test Loading (Single State)
```python
from etl.census.client import CensusClient
from etl.census.transform import transform_state_data
from etl.census.load import load_demographics
from etl.config import CENSUS_VARIABLES

client = CensusClient()
raw = client.fetch_county_data("56", CENSUS_VARIABLES)  # Wyoming
records = transform_state_data(raw)
stats = load_demographics(records)
print(stats)
```
- [ ] Records loaded without errors
- [ ] Database contains new data
- [ ] Commit: `git commit -m "feat: add Census data loading"`

---

## Day 5: Main Script and Testing

### Task 5.1: Create Run Script
Create `backend/etl/run_census.py`:
- [ ] Argument parsing (--state, --dry-run)
- [ ] Loop through all states
- [ ] Fetch, transform, load for each
- [ ] Progress logging
- [ ] Summary statistics
- [ ] Error handling
- [ ] Log to file

### Task 5.2: Add Logging Configuration
```python
import logging
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s',
    handlers=[
        logging.StreamHandler(),
        logging.FileHandler('etl_census.log')
    ]
)
```
- [ ] Console output shows progress
- [ ] Log file created with details

### Task 5.3: Run Full ETL
```bash
cd backend
source venv/bin/activate
python -m etl.run_census
```
- [ ] Script runs for all states
- [ ] No fatal errors
- [ ] 3,000+ records processed
- [ ] Summary shows counts

### Task 5.4: Verify Data
```sql
-- In psql
SELECT COUNT(*) FROM demographics;
SELECT
    l.name,
    l.state_name,
    d.median_household_income,
    d.population
FROM locations l
JOIN demographics d ON l.id = d.location_id
ORDER BY d.population DESC
LIMIT 10;
```
- [ ] Counts match expected
- [ ] Data looks reasonable

### Task 5.5: Spot Check
Pick 3-5 counties and verify against Census website:
- [ ] Go to https://data.census.gov
- [ ] Look up the same county
- [ ] Compare values (should be close, may differ by year)

---

## Day 6: Documentation and Cleanup

### Task 6.1: Add Docstrings
- [ ] All modules have module-level docstrings
- [ ] All functions have docstrings
- [ ] Complex logic has inline comments

### Task 6.2: Create ETL Documentation
Create `backend/etl/README.md`:
```markdown
# ETL Pipelines

## Census Data Pipeline

### Prerequisites
- Census API key in `.env`
- Locations table populated

### Running
```bash
python -m etl.run_census           # All states
python -m etl.run_census --state 06  # Single state
python -m etl.run_census --dry-run   # Test without loading
```

### Data Source
American Community Survey (ACS) 5-Year Estimates
https://www.census.gov/programs-surveys/acs
```

- [ ] Documentation complete

### Task 6.3: Run Idempotency Test
```bash
# Run twice - should produce same result
python -m etl.run_census --state 06
python -m etl.run_census --state 06

# Check counts are same
psql -U relocation_user -d relocation_advisor -c "SELECT COUNT(*) FROM demographics;"
```
- [ ] Second run doesn't create duplicates
- [ ] Second run updates existing records

### Task 6.4: Update Journal
Add Sprint 2 entry to `JOURNAL.md`:
- [ ] What you built
- [ ] Challenges with Census API
- [ ] Lessons learned about ETL
- [ ] Questions about data quality

### Task 6.5: Final Commit
```bash
git add .
git commit -m "feat: complete Census ETL pipeline"
git push
```
- [ ] All changes committed
- [ ] Repository clean

---

## Bonus: Pre-populate Locations

If you need to populate the locations table first:

```python
# backend/etl/census/fetch_locations.py
"""Fetch all US county locations from Census"""

def fetch_all_counties():
    """Fetch county names, FIPS codes, and basic info."""
    # Use Census API to get county list
    # https://api.census.gov/data/2022/acs/acs5?get=NAME,GEO_ID&for=county:*&in=state:*
    pass

def load_locations(records):
    """Load location records into database."""
    pass
```

This is extra work but may be necessary before demographics can be loaded.

---

*After completing all tasks, proceed to `../sprint-3-etl-additional/`*
