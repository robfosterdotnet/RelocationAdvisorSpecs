# Developer Tasks - Sprint 3

---

## Days 1-3: BLS Pipeline

### Day 1: Setup
- [ ] Get BLS API key from https://www.bls.gov/developers/
- [ ] Add to `.env`
- [ ] Create `etl/bls/` directory structure
- [ ] Test API with single series ID

### Day 2: BLS ETL
- [ ] Create `BLSClient` class
- [ ] Build series IDs for all counties
- [ ] Fetch unemployment data in batches
- [ ] Transform to our schema
- [ ] Handle missing counties

### Day 3: Load and Test
- [ ] Create load function for employment table
- [ ] Run full BLS ETL
- [ ] Verify data in database
- [ ] Fix any issues

---

## Days 4-5: Climate Pipeline

### Day 4: Data Acquisition
- [ ] Download NOAA Climate Normals (or get API key)
- [ ] Understand data format
- [ ] Create station-to-county mapping
- [ ] Create `etl/climate/` structure

### Day 5: Climate ETL
- [ ] Parse climate data (CSV or API)
- [ ] Transform to our schema
- [ ] Aggregate by county if needed
- [ ] Load into climate table
- [ ] Verify data

---

## Day 6: Verification and Documentation

### Verification
```sql
-- Check all tables
SELECT
    (SELECT COUNT(*) FROM demographics) as demo_count,
    (SELECT COUNT(*) FROM employment) as emp_count,
    (SELECT COUNT(*) FROM climate) as climate_count;

-- Check data quality
SELECT
    l.name, l.state_name,
    d.median_household_income,
    e.unemployment_rate,
    c.avg_temp_annual
FROM locations l
LEFT JOIN demographics d ON l.id = d.location_id
LEFT JOIN employment e ON l.id = e.location_id
LEFT JOIN climate c ON l.id = c.location_id
WHERE d.id IS NOT NULL
   OR e.id IS NOT NULL
   OR c.id IS NOT NULL
LIMIT 20;
```

### Documentation
- [ ] `etl/bls/README.md`
- [ ] `etl/climate/README.md`
- [ ] Update `JOURNAL.md`

### Final Commits
```bash
git add .
git commit -m "feat: add BLS and climate ETL pipelines"
git push
```

---

*After completion, proceed to `../sprint-4-core-api/`*
