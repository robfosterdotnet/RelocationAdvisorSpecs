# Technical Guidance - Sprint 1

**From:** Sarah Chen (Tech Lead)
**Subject:** Database Design Specifications

---

## Overview

This document provides design specifications and patterns for the database schema. You'll need to research the syntax and write the code yourself - that's how you'll learn.

---

## ER Diagram Tools

### Recommended: dbdiagram.io

1. Go to https://dbdiagram.io
2. Learn their DSL syntax from their documentation
3. Define all four tables with relationships
4. Export as PNG

### What Your Diagram Must Show
- All four tables with all columns
- Data types for each column
- Primary key notation (PK)
- Foreign key notation (FK)
- Relationship lines between tables

---

## SQL Schema Design Specifications

### File to Create: `database/migrations/001_initial_schema.sql`

### Locations Table Specification

| Column | Type | Constraints | Notes |
|--------|------|-------------|-------|
| id | Auto-increment integer | Primary key | PostgreSQL uses SERIAL |
| fips_code | String, 5 chars | Unique, Not Null | County FIPS code |
| name | String, 100 chars | Not Null | County name |
| state_name | String, 50 chars | Not Null | Full state name |
| state_fips | String, 2 chars | Not Null | State portion of FIPS |
| latitude | Floating point | Nullable | Geographic coordinate |
| longitude | Floating point | Nullable | Geographic coordinate |
| location_type | String, 20 chars | Default 'county' | Type of location |
| created_at | Timestamp | Default current time | When record was created |
| updated_at | Timestamp | Default current time | When record was last updated |

**Required indexes:** fips_code, state_fips

---

### Demographics Table Specification

| Column | Type | Constraints | Notes |
|--------|------|-------------|-------|
| id | Auto-increment integer | Primary key | |
| location_id | Integer | Foreign key to locations.id, Not Null | |
| median_household_income | Integer | Nullable | Annual income in USD |
| median_home_value | Integer | Nullable | Home price in USD |
| median_gross_rent | Integer | Nullable | Monthly rent in USD |
| population | Integer | Nullable | Total population |
| bachelors_degree_pct | Float | Nullable | Percentage (0-100) |
| data_year | Small integer | Nullable | Year data represents |
| created_at | Timestamp | Default current time | |
| updated_at | Timestamp | Default current time | |

**Required:** Foreign key with ON DELETE CASCADE, index on location_id

---

### Employment Table Specification

| Column | Type | Constraints | Notes |
|--------|------|-------------|-------|
| id | Auto-increment integer | Primary key | |
| location_id | Integer | Foreign key to locations.id, Not Null | |
| unemployment_rate | Float | Nullable | Percentage (0-100) |
| median_hourly_wage | Decimal(10,2) | Nullable | Wage in USD |
| job_growth_rate | Float | Nullable | Year-over-year percentage |
| labor_force | Integer | Nullable | Number of workers |
| data_year | Small integer | Nullable | Year data represents |
| created_at | Timestamp | Default current time | |
| updated_at | Timestamp | Default current time | |

**Required:** Foreign key with ON DELETE CASCADE, index on location_id

---

### Climate Table Specification

| Column | Type | Constraints | Notes |
|--------|------|-------------|-------|
| id | Auto-increment integer | Primary key | |
| location_id | Integer | Foreign key to locations.id, Not Null | |
| avg_temp_annual | Float | Nullable | Average yearly temp (F) |
| avg_temp_summer_high | Float | Nullable | July average high (F) |
| avg_temp_winter_low | Float | Nullable | January average low (F) |
| precipitation_annual | Float | Nullable | Yearly rainfall (inches) |
| snowfall_annual | Float | Nullable | Yearly snowfall (inches) |
| sunny_days | Small integer | Nullable | Clear days per year |
| created_at | Timestamp | Default current time | |
| updated_at | Timestamp | Default current time | |

**Required:** Foreign key with ON DELETE CASCADE, index on location_id

---

## SQL Patterns to Research

You'll need to learn these PostgreSQL concepts:

1. **CREATE TABLE syntax** - How to define tables with columns and types
2. **SERIAL** - PostgreSQL's auto-increment integer type
3. **CONSTRAINT** - How to name your constraints explicitly
4. **FOREIGN KEY ... REFERENCES** - How to create relationships
5. **ON DELETE CASCADE** - What happens when parent record is deleted
6. **CREATE INDEX** - How to add indexes for performance
7. **IF NOT EXISTS** - Making scripts safe to re-run
8. **DEFAULT CURRENT_TIMESTAMP** - Auto-setting timestamps

### Helpful Resources
- PostgreSQL CREATE TABLE: https://www.postgresql.org/docs/current/sql-createtable.html
- PostgreSQL Data Types: https://www.postgresql.org/docs/current/datatype.html

---

## SQLAlchemy Model Design Specifications

### File Structure
```
backend/app/models/
├── __init__.py      # Exports all models
├── base.py          # Base class and mixins
├── location.py      # Location model
├── demographics.py  # Demographics model
├── employment.py    # Employment model
└── climate.py       # Climate model
```

### Base Module Requirements (`base.py`)

1. Create a `Base` class using SQLAlchemy's `DeclarativeBase`
2. Create a `TimestampMixin` class that adds:
   - `created_at` column with auto-set default
   - `updated_at` column with auto-update on changes

### Model Requirements

Each model class must have:

1. **`__tablename__`** - Must match your SQL table name exactly
2. **All columns** - Matching your SQL schema (types, nullable, etc.)
3. **Relationships** - Using SQLAlchemy's `relationship()` function
4. **`__repr__` method** - For debugging output
5. **Docstring** - Explaining what the model represents

### Relationship Design

- Location has ONE demographics record (one-to-one)
- Location has ONE employment record (one-to-one)
- Location has ONE climate record (one-to-one)
- Use `uselist=False` for one-to-one relationships
- Use `back_populates` to create bidirectional relationships
- Use `cascade="all, delete-orphan"` for automatic cleanup

### Research Topics for SQLAlchemy

1. **DeclarativeBase** - SQLAlchemy 2.0 style base class
2. **Column types** - Integer, String, Float, etc.
3. **ForeignKey** - How to reference other tables
4. **relationship()** - Defining ORM relationships
5. **back_populates** - Bidirectional relationship linking

### Helpful Resources
- SQLAlchemy ORM Tutorial: https://docs.sqlalchemy.org/en/20/tutorial/
- Declaring Models: https://docs.sqlalchemy.org/en/20/orm/quickstart.html

---

## Database Connection Design (`database.py`)

Your database module should:

1. **Load configuration** from environment variables (DATABASE_URL)
2. **Create an engine** with connection pooling settings
3. **Create a session factory** (SessionLocal)
4. **Provide a `get_db()` function** that:
   - Creates a session
   - Yields it for use
   - Ensures cleanup (closes session)
5. **Provide an `init_db()` function** that creates all tables

### Configuration to Support
- `DATABASE_URL` - Full connection string
- `echo=True` option for SQL logging during development
- Connection pool settings (pool_size, max_overflow)

---

## Seed Data Design

### File: `database/seeds/seed_data.py`

Your seed script should:

1. **Connect to the database** using your session factory
2. **Clear existing data** (in correct order due to foreign keys)
3. **Insert 15-20 locations** with realistic data including:
   - Mix of locations: big cities, suburbs, rural areas
   - Variety of states across different regions
   - Real FIPS codes (look these up!)
4. **Insert related data** for each location (demographics, employment, climate)
5. **Handle errors** gracefully with rollback
6. **Print progress** so you know what's happening
7. **Be runnable from command line**

### Locations to Include (research real data for these)

Consider including diverse examples like:
- Major metros: Los Angeles CA, New York NY, Chicago IL
- Tech hubs: San Francisco CA, Seattle WA, Austin TX
- Affordable areas: Various counties in midwest/south
- Extreme climates: Phoenix AZ (hot), Minneapolis MN (cold)
- Small/rural: Several low-population counties

### Where to Find Real Data
- Census QuickFacts: https://www.census.gov/quickfacts/
- FIPS Code lookup: https://www.census.gov/library/reference/code-lists/ansi.html

---

## Testing Your Schema

### SQL Queries to Verify

Write queries that:
1. Select all locations with their related data (JOIN all tables)
2. Filter locations by state
3. Sort by various metrics (income, population, temperature)
4. Aggregate data (average income by state)
5. Use EXPLAIN to verify indexes are working

---

## Common Mistakes to Avoid

1. **Data type mismatches** - SQL schema and SQLAlchemy must match exactly
2. **Missing indexes** - Foreign key columns should be indexed
3. **Wrong cascade behavior** - Forgetting ON DELETE CASCADE
4. **Circular imports** - Be careful with model imports
5. **Forgetting to commit** - Database changes need explicit commit
6. **Not handling NULLs** - Many fields will have missing data

---

## Definition of "Done" for Code Review

When I review your code, I'll check:

1. Can you run the SQL script without errors?
2. Do the SQLAlchemy models match the SQL exactly?
3. Are relationships properly defined?
4. Can you query joined data successfully?
5. Does the seed script populate all tables?
6. Can you explain WHY you made each design choice?

---

*Next: Read `05-developer-tasks.md`*
