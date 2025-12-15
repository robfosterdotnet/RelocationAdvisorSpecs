# Learning Resources - Sprint 1

**Focus Areas:** Database Design, SQL, SQLAlchemy ORM

---

## Database Design Fundamentals

### Must Read
- [Database Design Basics](https://www.lucidchart.com/pages/database-diagram/database-design) - Visual guide
- [SQL Tutorial - W3Schools](https://www.w3schools.com/sql/) - Interactive basics

### Key Concepts to Understand

**1. Tables, Rows, and Columns**
- Table = a collection of related data
- Row = a single record (one location)
- Column = a single attribute (name, population, etc.)

**2. Primary Keys**
- Uniquely identifies each row
- Usually auto-incrementing integer
- Why: You need a way to reference specific records

**3. Foreign Keys**
- References a primary key in another table
- Creates relationships between tables
- Why: Connects demographics to their location

**4. Data Types**
```sql
INTEGER     -- Whole numbers: population, income
FLOAT       -- Decimal numbers: percentages, coordinates
VARCHAR(n)  -- Text up to n characters: names, codes
TEXT        -- Variable length text
TIMESTAMP   -- Date and time
BOOLEAN     -- True/False
```

### Practice Exercise
Before writing real SQL, practice at https://sqlbolt.com/ - complete lessons 1-6.

---

## Entity Relationship Diagrams

### Must Read
- [ER Diagram Tutorial](https://www.lucidchart.com/pages/er-diagrams) - Comprehensive guide

### Key Symbols
```
┌──────────────┐
│   TABLE      │
├──────────────┤
│ PK id        │  ← Primary Key
│    name      │
│ FK other_id  │  ← Foreign Key
└──────────────┘

Relationships:
─┤├─  One-to-Many
─┤├─  Many-to-Many
──||──  One-to-One
```

### Our Relationships
```
locations ──||── demographics   (one location has one demographics record)
locations ──||── employment     (one location has one employment record)
locations ──||── climate        (one location has one climate record)
```

---

## SQL Commands Reference

### Creating Tables
```sql
CREATE TABLE table_name (
    column1 datatype constraints,
    column2 datatype constraints,
    ...
    CONSTRAINT constraint_name constraint_definition
);
```

### Adding Constraints
```sql
-- Primary Key
id SERIAL PRIMARY KEY

-- Foreign Key
location_id INTEGER REFERENCES locations(id)

-- Unique
fips_code VARCHAR(5) UNIQUE

-- Not Null
name VARCHAR(100) NOT NULL

-- Default Value
created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
```

### Indexes
```sql
-- Create index for faster queries
CREATE INDEX idx_name ON table(column);

-- Check if index is used
EXPLAIN ANALYZE SELECT * FROM table WHERE column = 'value';
```

### Basic Queries
```sql
-- Select all
SELECT * FROM locations;

-- Select specific columns
SELECT name, state_name FROM locations;

-- Filter with WHERE
SELECT * FROM locations WHERE state_fips = '06';

-- Join tables
SELECT l.name, d.population
FROM locations l
JOIN demographics d ON l.id = d.location_id;

-- Sort
ORDER BY population DESC;

-- Limit
LIMIT 10;
```

---

## SQLAlchemy ORM

### Must Read
- [SQLAlchemy 2.0 Tutorial](https://docs.sqlalchemy.org/en/20/tutorial/) - First two sections
- [Declaring Models](https://docs.sqlalchemy.org/en/20/orm/quickstart.html) - Model syntax

### Key Concepts

**1. Declarative Base**
```python
from sqlalchemy.orm import DeclarativeBase

class Base(DeclarativeBase):
    pass
```

**2. Defining Models**
```python
from sqlalchemy import Column, Integer, String

class Location(Base):
    __tablename__ = 'locations'

    id = Column(Integer, primary_key=True)
    name = Column(String(100), nullable=False)
```

**3. Relationships**
```python
from sqlalchemy.orm import relationship

class Location(Base):
    demographics = relationship("Demographics", back_populates="location")

class Demographics(Base):
    location = relationship("Location", back_populates="demographics")
```

**4. Sessions**
```python
from sqlalchemy.orm import Session

with Session(engine) as session:
    location = session.query(Location).first()
    print(location.name)
```

---

## PostgreSQL Specifics

### psql Commands
```
\l          -- List databases
\c dbname   -- Connect to database
\dt         -- List tables
\d table    -- Describe table structure
\di         -- List indexes
\q          -- Quit
```

### PostgreSQL Data Types
```sql
SERIAL      -- Auto-incrementing integer
VARCHAR(n)  -- Variable string up to n chars
TEXT        -- Unlimited string
INTEGER     -- Whole number
FLOAT       -- Floating point
NUMERIC     -- Exact decimal (for money)
BOOLEAN     -- true/false
TIMESTAMP   -- Date and time
```

---

## FIPS Codes

### What They Are
- Federal Information Processing Standards codes
- Unique identifiers for geographic areas
- County FIPS = State FIPS (2) + County FIPS (3)

### Examples
```
06037 = California (06) + Los Angeles County (037)
36061 = New York (36) + New York County (061)
48201 = Texas (48) + Harris County (201)
```

### Resources
- [FIPS Code Lookup](https://www.census.gov/library/reference/code-lists/ansi.html)
- [Census QuickFacts](https://www.census.gov/quickfacts/) - Search any county

---

## Common Errors and Solutions

### "relation does not exist"
```
ERROR: relation "locations" does not exist
```
**Solution:** Table hasn't been created. Run your migration script.

### "duplicate key violates unique constraint"
```
ERROR: duplicate key value violates unique constraint "uq_locations_fips_code"
```
**Solution:** You're trying to insert a record with a FIPS code that already exists.

### "foreign key constraint fails"
```
ERROR: insert or update on table "demographics" violates foreign key constraint
```
**Solution:** The location_id you're referencing doesn't exist in the locations table.

### SQLAlchemy "no such column"
```
sqlalchemy.exc.OperationalError: no such column: locations.fips_code
```
**Solution:** Your model doesn't match your database schema. Check column names.

---

## Database Design Best Practices

### Naming
- Tables: lowercase, plural (`locations`, `demographics`)
- Columns: lowercase, snake_case (`median_income`, `data_year`)
- Constraints: descriptive (`fk_demographics_location`)

### Normalization (Simplified)
- **First Normal Form (1NF):** No repeating groups, each column has one value
- **Second Normal Form (2NF):** Every column depends on the whole primary key
- **Third Normal Form (3NF):** No transitive dependencies

For our project, keep it simple: one fact in one place.

### When to Index
- Primary keys (automatic)
- Foreign keys (important!)
- Columns you filter by often (WHERE clauses)
- Columns you sort by often (ORDER BY)

---

## Study Time Recommendations

| Topic | Time | Priority |
|-------|------|----------|
| SQL basics (SQLBolt lessons 1-6) | 2 hours | High |
| ER diagram concepts | 30 min | High |
| SQLAlchemy basics | 1.5 hours | High |
| PostgreSQL commands | 30 min | Medium |
| FIPS codes | 15 min | Medium |
| Normalization | 30 min | Low (for now) |

---

## Knowledge Check

Before moving on, you should be able to answer:

1. What is a primary key and why do we need it?
2. What does a foreign key do?
3. Why do we create indexes?
4. What's the difference between VARCHAR and TEXT?
5. What does ON DELETE CASCADE mean?
6. How do you create a one-to-one relationship in SQLAlchemy?
7. What is a FIPS code and how is it structured?

---

*Next: Read `07-definition-of-done.md`*
