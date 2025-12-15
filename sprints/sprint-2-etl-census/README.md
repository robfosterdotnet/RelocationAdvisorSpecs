# Sprint 2: ETL Pipeline - Census Data

**Duration:** 5-6 days
**Sprint Goal:** Build the first data pipeline to ingest Census ACS demographic data

---

## Sprint Overview

This is where the real data engineering begins. You'll learn how to:
- Work with government APIs
- Parse and clean messy real-world data
- Handle errors and edge cases
- Build reusable ETL patterns

**ETL = Extract, Transform, Load**
- **Extract:** Get data from the Census API
- **Transform:** Clean, normalize, and validate
- **Load:** Insert into our PostgreSQL database

---

## Why This Matters

> "Data is the new oil. But like oil, it's useless until you refine it."

Raw government data is:
- Inconsistently formatted
- Full of codes and abbreviations
- Missing values in unexpected places
- Updated on different schedules

Your job is to turn this messy reality into clean, queryable data. This skill is incredibly valuable in the job market.

---

## Sprint Contents

| Document | Purpose |
|----------|---------|
| `01-stakeholder-meeting.md` | Data requirements and priorities |
| `02-requirements.md` | ETL specifications |
| `03-user-stories.md` | Your assigned tasks |
| `04-technical-guidance.md` | Census API patterns and code examples |
| `05-developer-tasks.md` | Step-by-step implementation |
| `06-learning-resources.md` | APIs, data cleaning, Python skills |
| `07-definition-of-done.md` | Completion checklist |

---

## Sprint Deliverables

By the end of this sprint, you must have:

- [ ] Census API key obtained and configured
- [ ] ETL script that fetches demographic data for all US counties
- [ ] Data cleaning and validation logic
- [ ] FIPS code mapping to our locations table
- [ ] Successful population of demographics table
- [ ] Logging and error handling
- [ ] `JOURNAL.md` entry for Sprint 2

---

## Key Concepts You'll Learn

- **API Keys** - Authentication with external services
- **Rate Limiting** - Being a good API citizen
- **Data Validation** - Ensuring data quality
- **Error Handling** - Graceful failure and recovery
- **Idempotency** - Safe to run multiple times

---

## Start Here

Open `01-stakeholder-meeting.md` and begin reading.
