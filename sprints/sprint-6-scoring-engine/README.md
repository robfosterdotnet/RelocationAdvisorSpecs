# Sprint 6: Scoring Engine & Advanced API

**Duration:** 4-5 days
**Sprint Goal:** Build the weighted scoring system that ranks locations based on user preferences

---

## Sprint Overview

This is the "magic" of the application - turning raw data into personalized recommendations. You'll build:

- A scoring engine that normalizes data to comparable scales
- Weighted scoring based on user preferences
- Rankings and comparison endpoints
- The core value proposition of the product

---

## What You're Building

### Key Features
1. **Data Normalization** - Convert all metrics to 0-100 scale
2. **Weighted Scoring** - Let users weight what matters to them
3. **Rankings API** - Get top locations based on preferences
4. **Compare API** - Side-by-side location comparison

### Example Use Case
User says: "I care most about climate (40%), then affordability (30%), then jobs (20%), education (10%)"

The system:
1. Normalizes all metrics to 0-100
2. Applies weights to calculate composite score
3. Returns ranked list of best-matching locations

---

## Sprint Deliverables

- [ ] Scoring service with normalization
- [ ] `/api/v1/rankings` endpoint
- [ ] `/api/v1/compare` endpoint
- [ ] Preference schema validation
- [ ] Unit tests for scoring logic
- [ ] Journal entry for Sprint 6

---

## Sprint Contents

| Document | Purpose |
|----------|---------|
| `01-stakeholder-meeting.md` | Scoring requirements |
| `02-requirements.md` | Scoring specifications |
| `03-user-stories.md` | Assigned tasks |
| `04-technical-guidance.md` | Scoring algorithms |
| `05-developer-tasks.md` | Implementation |
| `06-learning-resources.md` | Data normalization |
| `07-definition-of-done.md` | Completion criteria |

---

## Start Here

Open `01-stakeholder-meeting.md` and begin reading.
