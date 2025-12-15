# Relocation Advisor - Junior Developer Training Project

## Welcome, Andrew!

Congratulations on graduating! This project is designed to give you real-world software development experience by simulating what it's like to work as a junior developer on an actual product team.

You'll be building the **Relocation Advisor** - a full-stack application that helps users find the best place to live based on their personal preferences.

---

## How This Project Works

### The Simulation

You've just been hired as a **Junior Software Developer** at a fictional company called **DataVista Solutions**. The company has won a contract to build a relocation advisory tool for a client.

- **Your Tech Lead**: Sarah Chen (simulated through documentation)
- **Product Owner**: Marcus Thompson (simulated through meeting minutes)
- **Your Role**: Junior Developer on the Relocation Advisor team

### What You'll Experience

Each sprint contains:
1. **Stakeholder Meeting Minutes** - What the business needs (you'll read these as if you attended)
2. **Requirements Document** - Formal specifications from the product team
3. **User Stories** - Your assigned work items with acceptance criteria
4. **Technical Guidance** - Architecture decisions and patterns to follow
5. **Developer Tasks** - Step-by-step implementation guide
6. **Learning Resources** - Links and concepts to study
7. **Code Review Checklist** - What your "tech lead" will look for

### The Rules

1. **Read everything before coding** - Just like a real job
2. **Follow the patterns provided** - Don't over-engineer or go rogue
3. **Commit often with good messages** - Your git history tells a story
4. **Ask questions** - Use the provided discussion prompts
5. **Document your decisions** - Keep a development journal
6. **Complete each sprint before moving on** - No skipping ahead

---

## Project Overview

### What You're Building

A web application where users can:
- Explore U.S. locations (counties/metros) on an interactive map
- Set their preferences (climate, cost of living, job market, etc.)
- Get personalized rankings of best places to live
- Compare multiple locations side-by-side
- View detailed data about any location

### Tech Stack (Decisions Made For You)

| Layer | Technology | Why |
|-------|------------|-----|
| Database | PostgreSQL | Industry standard, good for relational data |
| Backend | Python + FastAPI | Modern, fast, great for APIs |
| Frontend | React + TypeScript | Industry standard, employable skills |
| Visualization | Chart.js + Leaflet | Free, well-documented |
| Version Control | Git + GitHub | Industry standard |

### Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        FRONTEND (React)                         │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────────────┐   │
│  │   Map    │ │ Rankings │ │  Compare │ │  Location Detail │   │
│  └──────────┘ └──────────┘ └──────────┘ └──────────────────┘   │
└─────────────────────────────┬───────────────────────────────────┘
                              │ HTTP/JSON
┌─────────────────────────────▼───────────────────────────────────┐
│                      BACKEND (FastAPI)                          │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────────────┐   │
│  │ Location │ │ Rankings │ │  Scoring │ │    Preferences   │   │
│  │   API    │ │   API    │ │  Engine  │ │       API        │   │
│  └──────────┘ └──────────┘ └──────────┘ └──────────────────┘   │
└─────────────────────────────┬───────────────────────────────────┘
                              │ SQL
┌─────────────────────────────▼───────────────────────────────────┐
│                    DATABASE (PostgreSQL)                        │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────────────┐   │
│  │Locations │ │Demograph-│ │Employment│ │     Climate      │   │
│  │          │ │   ics    │ │          │ │                  │   │
│  └──────────┘ └──────────┘ └──────────┘ └──────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
                              ▲
                              │ ETL Scripts
┌─────────────────────────────┴───────────────────────────────────┐
│                     DATA SOURCES                                │
│  ┌──────────┐ ┌──────────┐ ┌──────────────────────────────┐    │
│  │  Census  │ │   BLS    │ │       NOAA Climate           │    │
│  │   ACS    │ │          │ │                              │    │
│  └──────────┘ └──────────┘ └──────────────────────────────┘    │
└─────────────────────────────────────────────────────────────────┘
```

---

## Sprint Schedule

| Sprint | Name | Focus | Duration |
|--------|------|-------|----------|
| 0 | Onboarding | Environment setup, Git, tooling | 3-4 days |
| 1 | Database Design | Schema design, PostgreSQL setup | 4-5 days |
| 2 | ETL: Census | First data pipeline, Census API | 5-6 days |
| 3 | ETL: Additional | BLS + Climate data integration | 5-6 days |
| 4 | Core API | FastAPI basics, CRUD endpoints | 5-6 days |
| 5 | Scoring Engine | Weighted scoring, rankings API | 4-5 days |
| 6 | Frontend Foundation | React setup, routing, components | 5-6 days |
| 7 | Frontend Features | Map, charts, interactive UI | 6-7 days |
| 8 | Integration & Testing | End-to-end testing, bug fixes | 4-5 days |
| 9 | Deployment & Docs | Docker, deployment, documentation | 4-5 days |

**Total Estimated Time: 8-10 weeks** (working ~4-6 hours/day)

---

## Getting Started

### Prerequisites

Before Sprint 0, make sure you have:
- [ ] A computer with macOS, Windows, or Linux
- [ ] A GitHub account
- [ ] Basic knowledge of Python and JavaScript (you'll learn the rest)
- [ ] Willingness to read documentation and Google things

### Start Here

1. Read this entire README
2. Go to `sprints/sprint-0-onboarding/`
3. Read the `README.md` in that folder
4. Follow the instructions

---

## Your Development Journal

Create a file called `JOURNAL.md` in the root of your project. After each sprint, write:

1. **What I built** - Summary of deliverables
2. **What I learned** - New concepts and skills
3. **What was hard** - Challenges you faced
4. **What I'd do differently** - Hindsight reflections
5. **Questions I have** - Things to research or ask about

This journal will be invaluable for interviews!

---

## Asking for Help

This is a self-guided project, but in a real job you'd ask questions. Here's how to simulate that:

1. **Google first** - Spend 15-20 minutes researching
2. **Check Stack Overflow** - See if others had similar issues
3. **Read the docs** - Official documentation is your friend
4. **Ask AI** - Use Claude or ChatGPT as a "senior developer"
5. **Ask Dad** - He's here to help!

When asking for help, practice good question-asking:
- What are you trying to do?
- What have you tried?
- What error or unexpected behavior are you seeing?
- What do you think might be wrong?

---

## Success Criteria

By the end of this project, you should be able to:

- [ ] Explain the entire system architecture
- [ ] Walk through any piece of code you wrote
- [ ] Discuss tradeoffs and decisions you made
- [ ] Demo the application end-to-end
- [ ] Talk about what you'd improve with more time
- [ ] Describe what you learned and how you grew

---

## One Final Note

**This is hard. It's supposed to be.**

Real software development involves:
- Reading documentation that doesn't quite answer your question
- Debugging errors that make no sense
- Making decisions without perfect information
- Refactoring code you thought was good
- Learning constantly

Don't get discouraged. Every professional developer has been where you are. The difference between juniors and seniors is just time and practice.

You've got this.

---

*Now go to `sprints/sprint-0-onboarding/` and begin your journey.*
