# Definition of Done - Sprint 2

**Sprint:** 2 - ETL Pipeline: Census Data

---

## Completion Checklist

### API Setup ✓
- [ ] Census API key obtained
- [ ] Key stored in `.env`
- [ ] `.env.example` updated
- [ ] Test request successful

### Code Structure ✓
- [ ] `backend/etl/config.py` exists
- [ ] `backend/etl/census/client.py` exists
- [ ] `backend/etl/census/transform.py` exists
- [ ] `backend/etl/census/load.py` exists
- [ ] `backend/etl/run_census.py` exists

### Functionality ✓
- [ ] Can fetch data from Census API
- [ ] Can transform raw data to our schema
- [ ] Can load data into database
- [ ] Script runs for all states
- [ ] Error handling works
- [ ] Logging to console and file

### Data Quality ✓
- [ ] 3,000+ counties in demographics table
- [ ] No obvious invalid data
- [ ] Spot-checked 5 counties against source
- [ ] Script is idempotent

### Documentation ✓
- [ ] `backend/etl/README.md` explains usage
- [ ] Functions have docstrings
- [ ] `JOURNAL.md` has Sprint 2 entry

---

## Verification Commands

```bash
# Test ETL for single state
python -m etl.run_census --state 56 --dry-run

# Run full ETL
python -m etl.run_census

# Check results
psql -U relocation_user -d relocation_advisor -c "
SELECT COUNT(*) as total,
       COUNT(median_household_income) as has_income,
       COUNT(population) as has_pop
FROM demographics;"
```

---

*Proceed to `../sprint-3-etl-additional/`*
