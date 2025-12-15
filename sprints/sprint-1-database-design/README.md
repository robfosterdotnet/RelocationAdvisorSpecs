# Sprint 1: Database Design

**Duration:** 4-5 days
**Sprint Goal:** Design and implement the database schema for the Relocation Advisor application

---

## Sprint Overview

This sprint is about designing the data model that will power the entire application. You'll learn:

- How to analyze requirements and translate them into database tables
- Entity-Relationship (ER) modeling
- Database normalization concepts
- Writing SQL to create tables
- Using SQLAlchemy for Python database interaction

---

## Why This Matters

> "Give me six hours to chop down a tree and I will spend the first four sharpening the axe." - Abraham Lincoln

A well-designed database schema:
- Makes queries simple and fast
- Prevents data inconsistencies
- Scales with growing data
- Makes future development easier

A poorly designed schema:
- Causes bugs that are hard to find
- Makes simple features complicated
- Requires painful migrations later
- Frustrates everyone

**Take your time with this sprint.** The rest of the project depends on these decisions.

---

## Sprint Contents

| Document | Purpose |
|----------|---------|
| `01-stakeholder-meeting.md` | Data requirements discussion |
| `02-requirements.md` | Database specifications |
| `03-user-stories.md` | Your assigned tasks |
| `04-technical-guidance.md` | Database design patterns |
| `05-developer-tasks.md` | Step-by-step implementation |
| `06-learning-resources.md` | Database design learning |
| `07-definition-of-done.md` | Completion checklist |

---

## Sprint Deliverables

By the end of this sprint, you must have:

- [ ] ER diagram documenting the data model
- [ ] SQL scripts to create all tables
- [ ] SQLAlchemy models matching the schema
- [ ] Database connection working from Python
- [ ] Seed data script for testing
- [ ] `JOURNAL.md` entry for Sprint 1

---

## Key Concepts You'll Learn

- **Primary Keys** - Unique identifiers for records
- **Foreign Keys** - Relationships between tables
- **Indexes** - Speed up queries
- **Normalization** - Organizing data to reduce redundancy
- **Data Types** - Choosing appropriate types for columns

---

## Start Here

Open `01-stakeholder-meeting.md` and begin reading.
