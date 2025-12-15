# Developer Tasks - Sprint 1

**Sprint:** 1 - Database Design
**Developer:** Andrew

---

## Day 1: Research and ER Diagram

### Task 1.1: Study FIPS Codes

- [ ] Read about FIPS codes: https://www.census.gov/library/reference/code-lists/ansi.html
- [ ] Understand structure: State (2 digits) + County (3 digits)
- [ ] Look up 5-10 example counties to use in seed data
- [ ] Note them in a text file for later

**Example FIPS codes to research:**
- 06037 - Los Angeles County, CA
- 36061 - New York County, NY (Manhattan)
- 17031 - Cook County, IL (Chicago)
- 48201 - Harris County, TX (Houston)
- 04013 - Maricopa County, AZ (Phoenix)

### Task 1.2: Create ER Diagram

- [ ] Go to https://dbdiagram.io (or your preferred tool)
- [ ] Create the four tables: locations, demographics, employment, climate
- [ ] Add all columns with data types
- [ ] Mark primary keys (PK) and foreign keys (FK)
- [ ] Draw relationship lines (one-to-one)
- [ ] Export as PNG
- [ ] Save to `docs/architecture/er-diagram.png`

### Task 1.3: Create Data Model Documentation

Create `docs/architecture/data-model.md`:

```markdown
# Relocation Advisor Data Model

## Overview
[Explain the overall data model in 2-3 paragraphs]

## Tables

### locations
[Explain purpose, key columns, relationships]

### demographics
[Explain purpose, data source, update frequency]

### employment
[Explain purpose, data source, update frequency]

### climate
[Explain purpose, data source, update frequency]

## Design Decisions
[Document WHY you made certain choices]
```

- [ ] Document created with all sections filled in

### Task 1.4: Commit Day 1 Work

```bash
git add docs/architecture/
git commit -m "docs: add ER diagram and data model documentation"
git push
```

- [ ] Committed and pushed

---

## Day 2: SQL Schema Development

### Task 2.1: Create Migration Directory Structure

```bash
cd database/migrations
touch 000_drop_all.sql
touch 001_initial_schema.sql
```

- [ ] Files created

### Task 2.2: Write Drop Script

Create `database/migrations/000_drop_all.sql`:

```sql
-- WARNING: This script drops ALL tables
-- Only use for development/testing

DROP TABLE IF EXISTS climate CASCADE;
DROP TABLE IF EXISTS employment CASCADE;
DROP TABLE IF EXISTS demographics CASCADE;
DROP TABLE IF EXISTS locations CASCADE;
```

- [ ] Drop script created

### Task 2.3: Write Initial Schema

Create `database/migrations/001_initial_schema.sql` with:
- [ ] Locations table with all columns
- [ ] Demographics table with foreign key
- [ ] Employment table with foreign key
- [ ] Climate table with foreign key
- [ ] All constraints named explicitly
- [ ] Indexes on foreign keys and fips_code
- [ ] Comments explaining each section

### Task 2.4: Test SQL Schema

```bash
# Connect to database
psql -U relocation_user -d relocation_advisor

# Run the schema script
\i database/migrations/001_initial_schema.sql

# Verify tables created
\dt

# Check table structure
\d locations
\d demographics

# Exit
\q
```

- [ ] Schema runs without errors
- [ ] All four tables exist
- [ ] Column types are correct

### Task 2.5: Commit Day 2 Work

```bash
git add database/
git commit -m "feat: add initial database schema migration"
git push
```

- [ ] Committed and pushed

---

## Day 3: SQLAlchemy Models

### Task 3.1: Create Base Model

Create `backend/app/models/base.py`:
- [ ] Import declarative_base from SQLAlchemy
- [ ] Create Base class
- [ ] Create TimestampMixin with created_at, updated_at
- [ ] Add docstrings

### Task 3.2: Create Location Model

Create `backend/app/models/location.py`:
- [ ] Import necessary SQLAlchemy components
- [ ] Define Location class extending Base and TimestampMixin
- [ ] Add all columns matching SQL schema
- [ ] Define relationships to other tables
- [ ] Add __repr__ method
- [ ] Add comprehensive docstring

### Task 3.3: Create Demographics Model

Create `backend/app/models/demographics.py`:
- [ ] Define Demographics class
- [ ] Add all columns
- [ ] Add foreign key to locations
- [ ] Add relationship back to Location
- [ ] Add __repr__ and docstring

### Task 3.4: Create Employment Model

Create `backend/app/models/employment.py`:
- [ ] Define Employment class
- [ ] Add all columns
- [ ] Add foreign key and relationship
- [ ] Add __repr__ and docstring

### Task 3.5: Create Climate Model

Create `backend/app/models/climate.py`:
- [ ] Define Climate class
- [ ] Add all columns
- [ ] Add foreign key and relationship
- [ ] Add __repr__ and docstring

### Task 3.6: Update Models __init__.py

Update `backend/app/models/__init__.py`:
- [ ] Import Base
- [ ] Import all four model classes
- [ ] Add __all__ list

### Task 3.7: Test Model Imports

```python
# In Python REPL (with venv activated)
>>> from app.models import Base, Location, Demographics, Employment, Climate
>>> print(Location.__tablename__)
'locations'
```

- [ ] All models import without error

### Task 3.8: Commit Day 3 Work

```bash
git add backend/app/models/
git commit -m "feat: add SQLAlchemy models for all tables"
git push
```

- [ ] Committed and pushed

---

## Day 4: Database Connection and Seed Data

### Task 4.1: Update Database Configuration

Update `backend/app/database.py`:
- [ ] Add DATABASE_URL from environment
- [ ] Create engine with connection pooling
- [ ] Create SessionLocal factory
- [ ] Add get_db() dependency function
- [ ] Add init_db() function
- [ ] Test connection

### Task 4.2: Test Database Connection

```python
# From backend directory with venv activated
>>> from app.database import engine
>>> from sqlalchemy import text
>>> with engine.connect() as conn:
...     result = conn.execute(text("SELECT 1"))
...     print(result.fetchone())
(1,)
```

- [ ] Connection works

### Task 4.3: Create Tables via SQLAlchemy

```python
>>> from app.database import init_db
>>> init_db()
```

- [ ] Tables created (or verified to exist)

### Task 4.4: Research Seed Data

Find realistic data for 15-20 US counties:
- [ ] 5 high-cost urban areas (NYC, LA, SF, etc.)
- [ ] 5 mid-cost suburban areas
- [ ] 5 lower-cost areas
- [ ] 5 diverse climate examples (hot, cold, moderate)

Sources for real data:
- Census QuickFacts: https://www.census.gov/quickfacts/
- BLS Area statistics: https://www.bls.gov/regions/
- Climate data: https://www.weather.gov/

### Task 4.5: Create Seed Data Script

Create `database/seeds/seed_data.py`:
- [ ] Import models and session
- [ ] Define SEED_LOCATIONS list with 15-20 entries
- [ ] Create seed_database() function
- [ ] Handle clearing existing data
- [ ] Insert all locations with related data
- [ ] Add error handling
- [ ] Make it runnable from command line

### Task 4.6: Run Seed Script

```bash
# From project root
cd backend
python -m database.seeds.seed_data
```

- [ ] Script runs without errors
- [ ] Data is inserted

### Task 4.7: Verify Seed Data

```sql
-- In psql
SELECT COUNT(*) FROM locations;
SELECT COUNT(*) FROM demographics;
SELECT l.name, d.median_household_income
FROM locations l
JOIN demographics d ON l.id = d.location_id;
```

- [ ] Correct number of records
- [ ] Data looks reasonable

### Task 4.8: Commit Day 4 Work

```bash
git add backend/app/database.py database/seeds/
git commit -m "feat: add database connection and seed data script"
git push
```

- [ ] Committed and pushed

---

## Day 5: Testing and Documentation

### Task 5.1: Create Sample Queries File

Create `docs/api/sample-queries.sql`:
- [ ] Query all locations with joined data
- [ ] Filter by state
- [ ] Sort by various metrics
- [ ] Aggregate queries (avg by state)
- [ ] EXPLAIN ANALYZE for index verification

### Task 5.2: Test All Sample Queries

```bash
psql -U relocation_user -d relocation_advisor
\i docs/api/sample-queries.sql
```

- [ ] All queries execute successfully
- [ ] Results look correct
- [ ] Indexes are being used (check EXPLAIN output)

### Task 5.3: Create Database README

Create `database/README.md`:

```markdown
# Database Documentation

## Setup

### Prerequisites
- PostgreSQL 15+
- Database `relocation_advisor` created
- User `relocation_user` with appropriate permissions

### Running Migrations

```bash
psql -U relocation_user -d relocation_advisor -f database/migrations/001_initial_schema.sql
```

### Seeding Data

```bash
cd backend
source venv/bin/activate
python -m database.seeds.seed_data
```

### Resetting Database

```bash
psql -U relocation_user -d relocation_advisor -f database/migrations/000_drop_all.sql
psql -U relocation_user -d relocation_advisor -f database/migrations/001_initial_schema.sql
```

## Schema Overview
[Brief description]

## Tables
[List tables and purposes]
```

- [ ] README created with all sections

### Task 5.4: Create Data Dictionary

Create `docs/data-dictionary/data-dictionary.md`:

Document each table and column:
```markdown
## locations

| Column | Type | Description | Source |
|--------|------|-------------|--------|
| id | SERIAL | Primary key | Auto-generated |
| fips_code | VARCHAR(5) | Federal FIPS code | Census |
...
```

- [ ] All tables documented
- [ ] All columns documented with sources

### Task 5.5: Update Journal

Add Sprint 1 entry to `JOURNAL.md`:
- [ ] What you built
- [ ] What you learned about databases
- [ ] Challenges (especially understanding FIPS, relationships, etc.)
- [ ] What you'd do differently
- [ ] Questions you have

### Task 5.6: Final Commits

```bash
git add .
git commit -m "docs: add database documentation and sample queries"
git push
```

- [ ] All changes committed
- [ ] Repository is clean (`git status` shows nothing)

---

## Sprint 1 Verification Checklist

Before moving on:

- [ ] ER diagram exists and is accurate
- [ ] SQL schema creates tables without errors
- [ ] SQLAlchemy models match SQL schema
- [ ] Database connection works from Python
- [ ] Seed data populates all tables
- [ ] Sample queries return expected results
- [ ] Indexes are verified with EXPLAIN
- [ ] Documentation is complete
- [ ] Journal entry is written
- [ ] All code is committed and pushed

---

*After completing all tasks, proceed to `../sprint-2-etl-census/`*
