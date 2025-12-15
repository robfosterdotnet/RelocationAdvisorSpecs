# Stakeholder Meeting Minutes

**Meeting:** Sprint 3 Planning - Additional Data Sources
**Attendees:** Marcus Thompson, Sarah Chen, David Park, Andrew

---

## Meeting Notes

### Marcus (Product Owner)

> "Great progress on Census data! Now let's complete our data foundation.
>
> **Employment Data (BLS):**
> Users care deeply about jobs. They want to know:
> - Can I find a job there? (unemployment rate)
> - Will it pay well? (median wages)
> - Is the economy growing? (job growth trends)
>
> **Climate Data (NOAA):**
> Climate is often the #1 factor for retirees and remote workers:
> - Is it too hot or too cold?
> - How much does it vary by season?
> - How much rain/snow?
> - How many sunny days?
>
> After this sprint, our MVP data layer is complete."

---

### Sarah (Tech Lead)

> "Two different APIs, two different challenges.
>
> **BLS API:**
> - Uses 'series IDs' instead of simple variable names
> - Example: LAUCN060370000000003 = LA County unemployment
> - Rate limited: 500 requests/day for registered users
> - Returns time series data (we'll take most recent)
>
> **NOAA API:**
> - Well-structured but data is by weather station, not county
> - Need to map stations to counties (or use county averages)
> - Alternative: Download NOAA Climate Normals CSV files
> - I recommend the CSV approach - simpler and more reliable
>
> **Mapping Challenge:**
> BLS uses MSA (Metropolitan Statistical Area) codes
> NOAA uses weather station IDs
> We use FIPS codes
>
> You'll need to create mapping tables or find crosswalks."

---

### David (Senior Developer)

> "Practical advice:
>
> **For BLS:**
> - Start with their sample queries
> - Request multiple series in one call (batch)
> - Cache responses - don't re-fetch same data
>
> **For Climate:**
> - Climate normals are 30-year averages - they don't change often
> - Consider downloading CSVs once vs. API calls
> - NOAA has pre-computed county-level summaries
>
> **Code Organization:**
> Follow the same pattern as Census:
> ```
> etl/bls/
> etl/climate/
> ```
>
> Reuse the patterns - don't reinvent the wheel."

---

## BLS Data Details

### Key Series
```
Unemployment Rate: LAUCNxxxxxxxxxx03
  - xx = State FIPS
  - xxxxxxxxx = County FIPS
  - 03 = Unemployment rate

Wages: OEUM{area}000000000001
  - {area} = Area code (MSA or state)
  - Need to map MSA to counties
```

### API Endpoint
```
https://api.bls.gov/publicAPI/v2/timeseries/data/
```

---

## NOAA Data Details

### Recommended Approach: Climate Normals CSVs
- Download from: https://www.ncei.noaa.gov/products/land-based-station/us-climate-normals
- Format: CSV with station data
- Coverage: 1991-2020 normals (most recent)

### Key Metrics
- Average annual temperature
- Average summer high (July)
- Average winter low (January)
- Annual precipitation
- Annual snowfall

---

## Action Items

| Owner | Action | Due |
|-------|--------|-----|
| Andrew | Get BLS API key | Day 1 |
| Andrew | BLS ETL pipeline | Day 1-3 |
| Andrew | Get NOAA data | Day 3-4 |
| Andrew | Climate ETL pipeline | Day 4-5 |
| Andrew | Verify all tables populated | Day 6 |

---

*Next: Read `02-requirements.md`*
