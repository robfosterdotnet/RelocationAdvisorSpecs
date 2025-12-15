# Definition of Done - Sprint 1

**Sprint:** 1 - Database Design

---

## Sprint 1 Completion Checklist

### ER Diagram ✓
- [ ] Diagram shows all four tables with correct names
- [ ] All columns listed with data types
- [ ] Primary keys marked (PK)
- [ ] Foreign keys marked (FK)
- [ ] Relationships shown correctly (one-to-one)
- [ ] Saved in `docs/architecture/er-diagram.png`

### SQL Schema ✓
- [ ] `database/migrations/001_initial_schema.sql` exists
- [ ] Creates all four tables
- [ ] All columns match specification
- [ ] Primary keys defined
- [ ] Foreign keys with ON DELETE CASCADE
- [ ] Indexes on foreign keys
- [ ] Index on fips_code
- [ ] Script runs without errors
- [ ] Script can be re-run (IF NOT EXISTS)

### SQLAlchemy Models ✓
- [ ] `backend/app/models/base.py` with Base class
- [ ] `backend/app/models/location.py` with Location class
- [ ] `backend/app/models/demographics.py` with Demographics class
- [ ] `backend/app/models/employment.py` with Employment class
- [ ] `backend/app/models/climate.py` with Climate class
- [ ] `backend/app/models/__init__.py` exports all models
- [ ] Column types match SQL schema
- [ ] Relationships defined with back_populates
- [ ] All models have docstrings
- [ ] All models have __repr__ method

### Database Connection ✓
- [ ] `backend/app/database.py` exists
- [ ] Reads DATABASE_URL from environment
- [ ] Creates engine with connection pooling
- [ ] get_db() function works
- [ ] init_db() creates tables

### Seed Data ✓
- [ ] `database/seeds/seed_data.py` exists
- [ ] Contains 15-20 real US counties
- [ ] Uses actual FIPS codes
- [ ] Includes realistic data values
- [ ] Script runs without errors
- [ ] Data appears in database after running

### Sample Queries ✓
- [ ] `docs/api/sample-queries.sql` exists
- [ ] Join query returns all data
- [ ] Filter by state works
- [ ] Sort by metrics works
- [ ] Aggregate query works
- [ ] EXPLAIN shows indexes being used

### Documentation ✓
- [ ] `docs/architecture/data-model.md` explains design
- [ ] `docs/data-dictionary/data-dictionary.md` documents all columns
- [ ] `database/README.md` explains setup
- [ ] `JOURNAL.md` has Sprint 1 entry

### Git ✓
- [ ] All code committed
- [ ] Meaningful commit messages
- [ ] Pushed to GitHub
- [ ] `git status` shows clean working directory

---

## Verification Commands

```bash
# Check database connection
psql -U relocation_user -d relocation_advisor -c "SELECT 1;"

# Verify tables exist
psql -U relocation_user -d relocation_advisor -c "\dt"

# Count records
psql -U relocation_user -d relocation_advisor -c "SELECT COUNT(*) FROM locations;"

# Test join
psql -U relocation_user -d relocation_advisor -c "
SELECT l.name, d.median_household_income
FROM locations l
JOIN demographics d ON l.id = d.location_id
LIMIT 5;"

# Test Python models
cd backend
source venv/bin/activate
python -c "from app.models import Location, Demographics; print('Models OK')"
python -c "from app.database import get_db; print('Database OK')"
```

---

## Demo Requirements

You should be able to demonstrate:

1. **ER Diagram** - Explain each table and relationship
2. **Run SQL migration** - Show it creates tables
3. **Python connection** - Query data from Python
4. **Sample queries** - Run and explain results

---

## Common Issues

### Models don't import
- Check `__init__.py` has all imports
- Check for circular imports
- Check model file names match class names

### Can't connect to database
- Is PostgreSQL running?
- Is DATABASE_URL correct in .env?
- Can you connect via psql?

### Foreign key errors
- Insert locations before related records
- Check location_id values exist

---

## Tech Lead Review Criteria

If Sarah were reviewing your work, she'd check:

1. **Does the schema match the spec?** - Column names, types, constraints
2. **Are relationships correct?** - One-to-one properly implemented
3. **Are indexes appropriate?** - Foreign keys, frequently queried columns
4. **Is the code clean?** - Consistent naming, good documentation
5. **Can you explain it?** - Why these tables? Why these data types?

---

*Congratulations on completing Sprint 1! Proceed to `../sprint-2-etl-census/`*
