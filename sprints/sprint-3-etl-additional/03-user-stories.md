# User Stories - Sprint 3

**Sprint:** 3 - ETL Pipeline: BLS & Climate

---

## Story S3-1: BLS API Setup (3 points)
Set up BLS API authentication and test connectivity.

### Acceptance Criteria
- [ ] BLS API key obtained
- [ ] Key stored in `.env`
- [ ] Test request successful

---

## Story S3-2: BLS ETL Pipeline (8 points)
Build complete BLS data pipeline.

### Acceptance Criteria
- [ ] `etl/bls/` module structure
- [ ] Fetch unemployment data
- [ ] Fetch wage data
- [ ] Transform to our schema
- [ ] Load into employment table
- [ ] 2,500+ counties populated

---

## Story S3-3: Climate Data Acquisition (3 points)
Obtain climate data from NOAA.

### Acceptance Criteria
- [ ] Climate normals data downloaded
- [ ] Data format understood
- [ ] Station-to-county mapping identified

---

## Story S3-4: Climate ETL Pipeline (8 points)
Build complete climate data pipeline.

### Acceptance Criteria
- [ ] `etl/climate/` module structure
- [ ] Parse CSV/JSON data
- [ ] Map to county FIPS
- [ ] Transform to our schema
- [ ] Load into climate table
- [ ] 2,500+ counties populated

---

## Story S3-5: Data Verification (3 points)
Verify all data tables are complete and accurate.

### Acceptance Criteria
- [ ] Demographics: 3,000+ counties
- [ ] Employment: 2,500+ counties
- [ ] Climate: 2,500+ counties
- [ ] Spot-check 10 counties
- [ ] No obvious data quality issues

---

## Story S3-6: Documentation (2 points)
Document both ETL pipelines.

### Acceptance Criteria
- [ ] BLS pipeline documented
- [ ] Climate pipeline documented
- [ ] Journal entry written

---

**Total Story Points:** 27

---

*Next: Read `04-technical-guidance.md`*
