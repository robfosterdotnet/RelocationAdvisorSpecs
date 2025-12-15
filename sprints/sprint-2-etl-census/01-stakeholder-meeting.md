# Stakeholder Meeting Minutes

**Meeting:** Sprint 2 Planning - Census Data Pipeline
**Date:** [Sprint 2 Start Date]
**Attendees:** Marcus Thompson, Sarah Chen, David Park, Andrew

---

## Meeting Notes

### Marcus (Product Owner)

> "Great work on the database design, Andrew. Now let's fill it with real data.
>
> **Priority Data from Census:**
>
> The Census Bureau's American Community Survey (ACS) has exactly what we need:
> - Median household income
> - Median home value
> - Median rent
> - Population
> - Educational attainment
>
> **What users care about:**
> - 'Can I afford to live there?' → Income vs home values/rent
> - 'What kind of place is it?' → Population, education levels
>
> Start with county-level data. We have ~3,100 counties in the US. That's manageable and gives good geographic coverage."

---

### Sarah (Tech Lead)

> "The Census API is well-documented but has quirks. Here's what you need to know:
>
> **Census API Basics:**
> - Free API key required (takes 5 minutes to get)
> - Rate limits are generous but be polite
> - Data is organized by 'tables' with codes like 'B19013' for median income
>
> **ACS Data Types:**
> - ACS 5-Year Estimates: Most reliable, available for all counties
> - We'll use the 5-year estimates for better data coverage
>
> **Technical Approach:**
> 1. Create an ETL script in `backend/etl/`
> 2. Fetch data in batches by state
> 3. Transform field names and clean values
> 4. Match to existing location records by FIPS
> 5. Upsert into demographics table
>
> **Key Variables (Table IDs):**
> ```
> B19013_001E - Median Household Income
> B25077_001E - Median Home Value
> B25064_001E - Median Gross Rent
> B01003_001E - Total Population
> B15003_022E - Bachelor's Degree
> B15003_001E - Total Education Population (for %)
> ```
>
> **Important:** The Census API returns string values, not numbers. You'll need to handle:
> - '-666666666' means 'data not available'
> - NULL or empty strings
> - Negative values in some fields"

---

### David (Senior Developer)

> "A few practical tips:
>
> **Script Structure:**
> ```
> etl/
> ├── __init__.py
> ├── census/
> │   ├── __init__.py
> │   ├── client.py      # API client
> │   ├── fetch.py       # Fetch functions
> │   ├── transform.py   # Data cleaning
> │   └── load.py        # Database insertion
> └── run_census.py      # Main entry point
> ```
>
> **Error Handling:**
> - Log everything - you'll thank yourself later
> - Don't fail on one bad record - skip and continue
> - Track how many records succeeded vs failed
>
> **Idempotency:**
> - Running the script twice should be safe
> - Use UPSERT (INSERT ... ON CONFLICT UPDATE) pattern
>
> **Testing:**
> - Start with one state (pick a small one like Wyoming)
> - Verify data before running for all states
> - Check a few records manually against Census website"

---

## Census API Overview

### Getting an API Key
1. Go to: https://api.census.gov/data/key_signup.html
2. Fill out the form
3. Check your email (usually instant)
4. Store the key in your `.env` file

### Base URL
```
https://api.census.gov/data/2022/acs/acs5
```

### Example API Call
```
https://api.census.gov/data/2022/acs/acs5?get=NAME,B19013_001E&for=county:*&in=state:06&key=YOUR_KEY
```

This returns: All California counties with median income

### Response Format
```json
[
  ["NAME", "B19013_001E", "state", "county"],
  ["Los Angeles County, California", "71358", "06", "037"],
  ["San Diego County, California", "89457", "06", "073"]
]
```

---

## Action Items

| Owner | Action | Due |
|-------|--------|-----|
| Andrew | Get Census API key | Day 1 |
| Andrew | Create ETL script structure | Day 1-2 |
| Andrew | Fetch and transform data | Day 2-4 |
| Andrew | Load into database | Day 4-5 |
| David | Code review ETL script | Day 5 |

---

## Key Takeaways for Andrew

1. **Get your API key first** - You can't do anything without it
2. **Start small** - Test with one state before running everything
3. **Handle edge cases** - Census data has quirks
4. **Log everything** - Future you will be grateful
5. **Make it idempotent** - Safe to run multiple times

---

*Next: Read `02-requirements.md`*
