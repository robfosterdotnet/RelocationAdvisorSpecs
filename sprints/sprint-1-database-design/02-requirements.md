# Requirements Document - Sprint 1

**Document Version:** 1.0
**Last Updated:** [Sprint 1 Start]
**Author:** Sarah Chen (Tech Lead)

---

## Overview

This document specifies the database schema requirements for the Relocation Advisor application.

---

## Functional Requirements

### FR-1.1: Location Master Table

The system must maintain a master table of U.S. locations with:
- Unique identifier (auto-generated)
- FIPS code (5-digit string for counties)
- Location name
- State name
- State FIPS code (2-digit)
- Geographic coordinates (latitude/longitude)
- Location type (county, metro area, etc.)

### FR-1.2: Demographics Table

The system must store demographic data including:
- Reference to location
- Median household income (USD)
- Median home value (USD)
- Median gross rent (USD)
- Total population
- Percentage with bachelor's degree or higher
- Data year (what year this data represents)

### FR-1.3: Employment Table

The system must store employment data including:
- Reference to location
- Unemployment rate (percentage)
- Median hourly wage (USD)
- Job growth rate (percentage, year-over-year)
- Labor force size
- Data year

### FR-1.4: Climate Table

The system must store climate data including:
- Reference to location
- Average annual temperature (Fahrenheit)
- Average summer high temperature
- Average winter low temperature
- Annual precipitation (inches)
- Annual snowfall (inches)
- Number of sunny days per year

### FR-1.5: Data Integrity

- All tables must have primary keys
- Foreign keys must reference the locations table
- FIPS codes must be unique in the locations table
- Appropriate indexes must be created for query performance

---

## Non-Functional Requirements

### NFR-1.1: Data Types

| Data Type | Use For | Example |
|-----------|---------|---------|
| SERIAL | Auto-increment IDs | id |
| VARCHAR(n) | Text with max length | fips_code VARCHAR(5) |
| TEXT | Variable length text | name |
| INTEGER | Whole numbers | population |
| DECIMAL(10,2) | Money values | median_income |
| FLOAT | Percentages, coordinates | unemployment_rate |
| SMALLINT | Data year | data_year |
| TIMESTAMP | Timestamps | created_at |

### NFR-1.2: Naming Conventions

- Tables: lowercase, plural, snake_case
- Columns: lowercase, snake_case
- Indexes: `idx_<table>_<column(s)>`
- Foreign keys: `fk_<table>_<referenced_table>`

### NFR-1.3: Required Metadata Columns

Every table must include:
- `created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP`
- `updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP`

### NFR-1.4: Performance

- Index all foreign key columns
- Index FIPS codes in locations table
- Index frequently queried columns (to be determined by query patterns)

---

## Database Schema Specification

### Table: locations

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | SERIAL | PRIMARY KEY | Auto-increment ID |
| fips_code | VARCHAR(5) | UNIQUE, NOT NULL | County FIPS code |
| name | VARCHAR(100) | NOT NULL | County/location name |
| state_name | VARCHAR(50) | NOT NULL | Full state name |
| state_fips | VARCHAR(2) | NOT NULL | State FIPS code |
| latitude | FLOAT | | Geographic latitude |
| longitude | FLOAT | | Geographic longitude |
| location_type | VARCHAR(20) | DEFAULT 'county' | Type of location |
| created_at | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP | Record created |
| updated_at | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP | Record updated |

### Table: demographics

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | SERIAL | PRIMARY KEY | Auto-increment ID |
| location_id | INTEGER | FOREIGN KEY, NOT NULL | Reference to locations |
| median_household_income | INTEGER | | Annual income in USD |
| median_home_value | INTEGER | | Home value in USD |
| median_gross_rent | INTEGER | | Monthly rent in USD |
| population | INTEGER | | Total population |
| bachelors_degree_pct | FLOAT | | % with bachelor's+ |
| data_year | SMALLINT | | Year data represents |
| created_at | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP | Record created |
| updated_at | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP | Record updated |

### Table: employment

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | SERIAL | PRIMARY KEY | Auto-increment ID |
| location_id | INTEGER | FOREIGN KEY, NOT NULL | Reference to locations |
| unemployment_rate | FLOAT | | Unemployment % |
| median_hourly_wage | DECIMAL(10,2) | | Median wage in USD |
| job_growth_rate | FLOAT | | YoY job growth % |
| labor_force | INTEGER | | Size of labor force |
| data_year | SMALLINT | | Year data represents |
| created_at | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP | Record created |
| updated_at | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP | Record updated |

### Table: climate

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | SERIAL | PRIMARY KEY | Auto-increment ID |
| location_id | INTEGER | FOREIGN KEY, NOT NULL | Reference to locations |
| avg_temp_annual | FLOAT | | Average annual temp (F) |
| avg_temp_summer_high | FLOAT | | Avg summer high (F) |
| avg_temp_winter_low | FLOAT | | Avg winter low (F) |
| precipitation_annual | FLOAT | | Annual precip (inches) |
| snowfall_annual | FLOAT | | Annual snow (inches) |
| sunny_days | SMALLINT | | Days of sunshine/year |
| created_at | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP | Record created |
| updated_at | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP | Record updated |

---

## Entity Relationship Diagram Requirements

Create an ER diagram showing:
- All four tables
- Primary keys (PK notation)
- Foreign keys (FK notation)
- Relationships (one-to-one in this case)
- Column names and types

Use any tool: draw.io, Lucidchart, dbdiagram.io, or even hand-drawn.

---

## Acceptance Criteria

1. SQL script creates all tables without errors
2. All constraints are properly defined
3. Indexes exist on all foreign keys
4. SQLAlchemy models match the SQL schema exactly
5. Can insert and query test data successfully
6. ER diagram accurately reflects the schema

---

## Out of Scope

- Historical data tracking (versioning)
- User data tables
- Preference storage
- Caching tables

---

*Next: Read `03-user-stories.md`*
