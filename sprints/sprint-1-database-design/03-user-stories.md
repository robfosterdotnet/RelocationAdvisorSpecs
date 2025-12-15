# User Stories - Sprint 1

**Sprint:** 1 - Database Design
**Assigned To:** Andrew [Your Last Name]

---

## Story S1-1: Create ER Diagram

**As a** developer
**I want** a visual diagram of the database schema
**So that** I can understand and communicate the data model

### Acceptance Criteria
- [ ] Diagram shows all four tables (locations, demographics, employment, climate)
- [ ] All columns are listed with data types
- [ ] Primary keys are marked (PK)
- [ ] Foreign keys are marked (FK)
- [ ] Relationships are shown with proper notation
- [ ] Diagram is saved in `docs/architecture/er-diagram.png` (or .pdf)
- [ ] A markdown file `docs/architecture/data-model.md` explains the design

### Story Points: 3

### Notes from Tech Lead (Sarah)
> "Start with the diagram before writing any SQL. I recommend dbdiagram.io - it's free and exports nice images. Think about the relationships: each demographic record belongs to one location, each location has one demographic record (one-to-one). This is simpler than many database designs but it's appropriate for our use case."

---

## Story S1-2: Write SQL Schema Script

**As a** developer
**I want** SQL scripts that create the database schema
**So that** I can version control and reproduce the database structure

### Acceptance Criteria
- [ ] Script creates all four tables with correct columns
- [ ] Primary keys are defined
- [ ] Foreign keys are defined with ON DELETE CASCADE
- [ ] Indexes are created on foreign keys and fips_code
- [ ] Script is idempotent (can be run multiple times safely)
- [ ] Script is saved in `database/migrations/001_initial_schema.sql`
- [ ] Script can be run successfully against PostgreSQL

### Story Points: 5

### Notes from Tech Lead (Sarah)
> "Use IF NOT EXISTS to make the script re-runnable. Include comments explaining what each section does. Also create a `000_drop_all.sql` script for resetting the database during development - but be careful with that one! Name your constraints explicitly rather than letting PostgreSQL auto-generate names."

---

## Story S1-3: Create SQLAlchemy Models

**As a** developer
**I want** Python SQLAlchemy models for each table
**So that** I can interact with the database using Python objects

### Acceptance Criteria
- [ ] Model class exists for each table
- [ ] Column definitions match SQL schema exactly
- [ ] Relationships are defined between models
- [ ] Models are in `backend/app/models/` directory
- [ ] Models are importable from `backend/app/models/__init__.py`
- [ ] Each model has a docstring explaining its purpose
- [ ] Models include `__repr__` method for debugging

### Story Points: 5

### Notes from Tech Lead (Sarah)
> "Follow SQLAlchemy 2.0 patterns. Use type hints. Define relationships with `relationship()` and `back_populates`. Create a `base.py` with your declarative base class. Each model should be in its own file: `location.py`, `demographics.py`, etc."

---

## Story S1-4: Database Connection Setup

**As a** developer
**I want** a working database connection from Python
**So that** the application can communicate with PostgreSQL

### Acceptance Criteria
- [ ] Connection string is read from environment variables
- [ ] `database.py` creates engine and session factory
- [ ] Can connect to database and execute simple query
- [ ] Connection pooling is configured
- [ ] Error handling for connection failures
- [ ] Connection is tested on application startup

### Story Points: 3

### Notes from Tech Lead (Sarah)
> "Use `create_engine` with `echo=True` during development to see SQL queries. Create a `get_db()` dependency function that yields sessions - we'll use this with FastAPI later. Make sure to close connections properly."

---

## Story S1-5: Create Seed Data Script

**As a** developer
**I want** a script that populates the database with test data
**So that** I can test queries and verify the schema works

### Acceptance Criteria
- [ ] Script creates 15-20 location records with realistic data
- [ ] Each location has associated demographics, employment, and climate data
- [ ] Data represents real US counties (use actual FIPS codes)
- [ ] Script can be run multiple times (clears and re-inserts)
- [ ] Script is in `database/seeds/seed_data.py`
- [ ] Script can be run from command line

### Story Points: 3

### Notes from Tech Lead (Sarah)
> "Pick a mix of locations: some big cities, some suburbs, some rural areas. Use real FIPS codes from Census.gov. The data doesn't have to be 100% accurate - we just need realistic values for testing. Include edge cases: high cost of living vs. low, hot climate vs. cold, etc."

---

## Story S1-6: Verify Schema with Queries

**As a** developer
**I want** to verify the schema by running test queries
**So that** I know the database design supports our application needs

### Acceptance Criteria
- [ ] Can query locations with all related data (JOIN)
- [ ] Can filter locations by state
- [ ] Can sort locations by any metric (income, temperature, etc.)
- [ ] Can aggregate data (average income by state)
- [ ] Queries perform well with indexes
- [ ] Test queries are documented in `docs/api/sample-queries.sql`

### Story Points: 2

### Notes from Tech Lead (Sarah)
> "Write queries for common operations: 'find all locations in Texas', 'find top 10 by median income', 'find locations where average temp is between 60-75'. These will inform our API design in Sprint 4. Use EXPLAIN to check that indexes are being used."

---

## Story S1-7: Documentation Update

**As a** developer
**I want** updated documentation for the database layer
**So that** future developers can understand the data model

### Acceptance Criteria
- [ ] ER diagram is in `docs/architecture/`
- [ ] Data dictionary exists explaining each field
- [ ] README in `database/` folder explains setup
- [ ] Journal entry for Sprint 1 is written
- [ ] Any deviations from spec are documented

### Story Points: 2

### Notes from Tech Lead (Sarah)
> "Create a data dictionary - a document that explains what each column means, where the data comes from, and any important notes. For example: 'median_household_income: Annual household income in USD from Census ACS 5-year estimates. NULL if data unavailable.'"

---

## Sprint Velocity Target

**Total Story Points:** 23

This is more complex than Sprint 0. Take your time understanding database concepts.

---

## Story Dependencies

```
S1-1 (ER Diagram)
    ↓
S1-2 (SQL Schema)
    ↓
S1-3 (SQLAlchemy Models) ← S1-4 (DB Connection)
    ↓
S1-5 (Seed Data)
    ↓
S1-6 (Test Queries)
    ↓
S1-7 (Documentation)
```

---

## Definition of Ready

A story is ready when:
- Previous dependencies are complete
- You understand what's being asked
- You have access to necessary resources
- You've reviewed the technical guidance

## Definition of Done

A story is done when:
- All acceptance criteria are met
- Code is committed with meaningful messages
- No database errors when running scripts
- You can explain your design choices

---

*Next: Read `04-technical-guidance.md`*
