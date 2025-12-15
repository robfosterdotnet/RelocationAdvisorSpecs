# Sprint 3: ETL Pipeline - BLS & Climate Data

**Duration:** 5-6 days
**Sprint Goal:** Complete the data layer by adding employment (BLS) and climate (NOAA) data

---

## Sprint Overview

You've already built one ETL pipeline. Now you'll build two more, faster, using the patterns you learned. This sprint tests your ability to:

- Apply existing patterns to new data sources
- Handle different API structures and data formats
- Integrate multiple data sources into a unified schema

---

## Data Sources

### Bureau of Labor Statistics (BLS)
- **What:** Employment data (unemployment, wages, job growth)
- **API:** https://www.bls.gov/developers/
- **Format:** JSON API with series codes
- **Key:** Required (free, instant signup)

### NOAA Climate Data
- **What:** Historical climate averages
- **API:** https://www.ncdc.noaa.gov/cdo-web/webservices/v2
- **Format:** JSON API
- **Key:** Required (free signup)
- **Alternative:** CSV downloads from climate normals

---

## Sprint Deliverables

- [ ] BLS API integration and data loading
- [ ] Climate data integration and loading
- [ ] Employment table populated for all counties
- [ ] Climate table populated for all counties
- [ ] Documentation for both pipelines
- [ ] Journal entry for Sprint 3

---

## Sprint Contents

| Document | Purpose |
|----------|---------|
| `01-stakeholder-meeting.md` | Data source discussion |
| `02-requirements.md` | ETL specifications for both sources |
| `03-user-stories.md` | Assigned tasks |
| `04-technical-guidance.md` | BLS and NOAA API patterns |
| `05-developer-tasks.md` | Implementation checklist |
| `06-learning-resources.md` | Working with multiple APIs |
| `07-definition-of-done.md` | Completion criteria |

---

## Key Challenge

**Different data granularities:** BLS data might be at metro level, Climate data at weather station level. You'll need to map these to our county-level locations. This is a real-world data engineering challenge!

---

## Start Here

Open `01-stakeholder-meeting.md` and begin reading.
