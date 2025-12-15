# Stakeholder Meeting Minutes

**Meeting:** Sprint 1 Planning - Data Model Discussion
**Date:** [Sprint 1 Start Date]
**Attendees:**
- Marcus Thompson (Product Owner)
- Sarah Chen (Tech Lead)
- David Park (Senior Developer)
- Andrew [Your Last Name] (Junior Developer)

---

## Meeting Notes

### Marcus (Product Owner) - Data Requirements

> "Let's talk about the data we need for this application. I've done some research on what our users care about when relocating.
>
> **Primary Data Points:**
>
> 1. **Location Information**
>    - We need to identify places uniquely - FIPS codes are the government standard
>    - County-level data is our target granularity (3,000+ counties in the US)
>    - We'll want to know the state, name, and geographic coordinates
>
> 2. **Demographics & Cost of Living**
>    - Median household income - how much do people make?
>    - Median home value - how expensive is housing?
>    - Median rent - for people who rent
>    - Population - is it a big city or rural?
>    - Education levels - percentage with bachelor's degrees
>
> 3. **Employment**
>    - Unemployment rate - how hard is it to find a job?
>    - Median wage - what do jobs pay?
>    - Job growth rate - is the economy growing or shrinking?
>
> 4. **Climate**
>    - Average temperature - is it hot or cold?
>    - Temperature variance - is it consistent or variable?
>    - Precipitation - how much rain/snow?
>
> **Future Considerations (not for MVP):**
> - Crime rates
> - Healthcare access
> - School quality
> - Commute times
>
> Let's design for these four core categories first."

---

### Sarah (Tech Lead) - Technical Direction

> "Thanks Marcus. Andrew, here's my thinking on the database design:
>
> **Core Design Decisions:**
>
> 1. **Separate tables for each data category**
>    - Locations (master table)
>    - Demographics
>    - Employment
>    - Climate
>
>    Why? Different data sources update at different frequencies. Census might update annually, climate data might be historical averages. Separating them makes updates cleaner.
>
> 2. **FIPS codes as the joining key**
>    - FIPS (Federal Information Processing Standards) codes uniquely identify geographic areas
>    - County FIPS codes are 5 digits: 2 for state + 3 for county
>    - Example: Los Angeles County, CA = '06037' (California is 06, LA County is 037)
>
> 3. **Nullable columns**
>    - Not every data source will have data for every county
>    - Design for missing data from the start
>
> 4. **Timestamps**
>    - Add `created_at` and `updated_at` to track data freshness
>    - Add `data_year` to know what year the data represents
>
> **What I want from you:**
>
> Andrew, I want you to:
> - Create an ER diagram before writing any code
> - Write the raw SQL first, then translate to SQLAlchemy
> - Think about data types carefully (INTEGER vs FLOAT vs DECIMAL)
> - Consider what queries we'll need to run and add indexes accordingly
>
> Don't just copy my table designs blindly - understand why each decision was made. I might ask you to explain your choices in our code review."

---

### David (Senior Developer) - Practical Advice

> "A few tips from experience:
>
> **Naming Conventions:**
> - Table names: lowercase, plural, snake_case (`locations`, `demographics`)
> - Column names: lowercase, snake_case (`median_income`, `unemployment_rate`)
> - Foreign keys: `<singular_table>_id` (`location_id`)
>
> **Common Mistakes to Avoid:**
> - Don't use FLOAT for money - use INTEGER cents or DECIMAL
> - Don't forget NOT NULL constraints where appropriate
> - Don't skip indexes on foreign keys
> - Don't create tables without testing them first
>
> **For Andrew:**
> When you create your SQLAlchemy models, I'll be reviewing them. I'll be looking for:
> - Proper relationship definitions
> - Matching column types to SQL schema
> - Good docstrings explaining what each model represents
>
> Also, create some seed data so we can test queries. Don't need thousands of records - 10-20 locations with complete data is enough for Sprint 1."

---

## Data Source Overview

| Data | Source | Geographic Level | Update Frequency |
|------|--------|-----------------|------------------|
| Demographics | Census ACS | County | Annual |
| Employment | BLS | County/Metro | Monthly |
| Climate | NOAA | Weather Stations | Historical |

---

## Action Items

| Owner | Action | Due |
|-------|--------|-----|
| Andrew | Create ER diagram | Day 2 |
| Andrew | Write SQL schema | Day 3 |
| Andrew | Create SQLAlchemy models | Day 4 |
| Andrew | Create seed data script | Day 5 |
| Sarah | Review ER diagram | Day 2 |
| David | Code review SQLAlchemy models | Day 5 |

---

## Questions Raised

**Q: Should we store raw data or normalized scores?**
Sarah: Store raw data. We'll calculate scores dynamically. This gives us flexibility to adjust scoring algorithms without re-importing data.

**Q: What about historical data - should we track changes over time?**
Marcus: Not for MVP. We'll just store the most recent data. Historical tracking is a future feature.

**Q: Should we include state-level summaries?**
Sarah: Let's start with county-level only. State summaries can be calculated from county data.

---

## Key Takeaways for Andrew

1. **Start with an ER diagram** - Visual design before code
2. **FIPS codes are important** - They're how we link different data sources
3. **Design for missing data** - NULL values will be common
4. **Think about queries** - What questions will the application need to answer?
5. **Separate concerns** - Different data categories in different tables

---

*Next: Read `02-requirements.md`*
