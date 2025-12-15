# Developer Tasks - Sprint 0

**Sprint:** 0 - Onboarding & Environment Setup
**Developer:** Andrew

---

## How to Use This Document

This is your step-by-step checklist. Work through each task in order. Check off items as you complete them. If you get stuck, refer back to the Technical Guidance document.

---

## Day 1: Git and Repository Setup

### Task 1.1: Install Git (if not already installed)

- [ ] Open terminal/command prompt
- [ ] Run `git --version`
- [ ] If not installed, install from https://git-scm.com/downloads
- [ ] Verify installation with `git --version`

### Task 1.2: Configure Git Identity

- [ ] Run `git config --global user.name "Your Full Name"`
- [ ] Run `git config --global user.email "your.email@example.com"`
- [ ] Verify with `git config --list`

### Task 1.3: Create GitHub Repository

- [ ] Log in to GitHub (create account if needed)
- [ ] Click "New Repository" (+ icon in top right)
- [ ] Name it `relocation-advisor`
- [ ] Set to Public (or Private - your choice)
- [ ] Do NOT initialize with README, .gitignore, or license
- [ ] Click "Create Repository"
- [ ] Copy the repository URL

### Task 1.4: Clone and Initialize

```bash
# Clone the empty repository
git clone https://github.com/YOUR_USERNAME/relocation-advisor.git
cd relocation-advisor

# Create initial README
echo "# Relocation Advisor" > README.md
echo "A data-driven tool to help users find the best place to live." >> README.md

# First commit
git add README.md
git commit -m "chore: initialize repository with README"
git push origin main
```

- [ ] Repository cloned successfully
- [ ] README created and committed
- [ ] Pushed to GitHub (verify on github.com)

### Task 1.5: Create .gitignore

- [ ] Create `.gitignore` file (copy from Technical Guidance)
- [ ] Commit: `git add .gitignore && git commit -m "chore: add gitignore"`
- [ ] Push: `git push`

---

## Day 2: Python Environment

### Task 2.1: Install Python

- [ ] Check current version: `python --version` or `python3 --version`
- [ ] If below 3.11, install Python 3.11+ (see Technical Guidance)
- [ ] Verify: `python --version` shows 3.11+

### Task 2.2: Create Project Structure - Backend

```bash
# From project root
mkdir -p backend/app/models
mkdir -p backend/app/schemas
mkdir -p backend/app/routers
mkdir -p backend/app/services
mkdir -p backend/etl
mkdir -p backend/tests

# Create Python package files
touch backend/app/__init__.py
touch backend/app/models/__init__.py
touch backend/app/schemas/__init__.py
touch backend/app/routers/__init__.py
touch backend/app/services/__init__.py
touch backend/etl/__init__.py
touch backend/tests/__init__.py
```

- [ ] All directories created
- [ ] All `__init__.py` files created

### Task 2.3: Create Virtual Environment

```bash
cd backend
python -m venv venv

# Activate (Mac/Linux)
source venv/bin/activate

# Activate (Windows)
.\venv\Scripts\activate
```

- [ ] Virtual environment created
- [ ] Virtual environment activated (terminal shows `(venv)`)

### Task 2.4: Install Initial Dependencies

```bash
# Make sure venv is activated!
pip install --upgrade pip
pip install fastapi uvicorn sqlalchemy psycopg2-binary python-dotenv pytest

# Save dependencies
pip freeze > requirements.txt
```

- [ ] Packages installed without errors
- [ ] `requirements.txt` created

### Task 2.5: Create Placeholder Files

Create `backend/app/main.py`:
```python
"""
Relocation Advisor API - Main Entry Point
"""
from fastapi import FastAPI

app = FastAPI(
    title="Relocation Advisor API",
    description="API for the Relocation Advisor application",
    version="0.1.0"
)

@app.get("/")
async def root():
    return {"message": "Relocation Advisor API", "status": "running"}

@app.get("/health")
async def health_check():
    return {"status": "healthy"}
```

- [ ] `main.py` created with placeholder code

### Task 2.6: Test Python Setup

```bash
# From backend directory, with venv activated
uvicorn app.main:app --reload

# Open browser to http://localhost:8000
# Should see JSON response

# Also check http://localhost:8000/docs
# Should see Swagger UI
```

- [ ] Server starts without errors
- [ ] Browser shows JSON response
- [ ] Swagger docs load at /docs

### Task 2.7: Commit Backend Setup

```bash
cd ..  # Back to project root
git add backend/
git commit -m "feat: add backend project structure and FastAPI setup"
git push
```

- [ ] Backend committed and pushed

---

## Day 3: Node.js and PostgreSQL

### Task 3.1: Install Node.js

- [ ] Check current version: `node --version`
- [ ] If not installed or below 18, install Node.js 18+ LTS
- [ ] Verify: `node --version` shows 18+
- [ ] Verify npm: `npm --version`

### Task 3.2: Create Frontend Structure

```bash
# From project root
mkdir -p frontend/src/components
mkdir -p frontend/src/pages
mkdir -p frontend/src/services
mkdir -p frontend/src/hooks
mkdir -p frontend/src/types
mkdir -p frontend/src/utils
mkdir -p frontend/public
```

- [ ] All directories created

### Task 3.3: Initialize Frontend (Placeholder)

Create `frontend/package.json`:
```json
{
  "name": "relocation-advisor-frontend",
  "version": "0.1.0",
  "description": "Frontend for Relocation Advisor",
  "private": true,
  "scripts": {
    "placeholder": "echo Frontend will be set up in Sprint 6"
  }
}
```

Create `frontend/README.md`:
```markdown
# Relocation Advisor Frontend

React frontend for the Relocation Advisor application.

## Setup

Frontend setup will be completed in Sprint 6.
```

- [ ] `package.json` created
- [ ] Frontend README created

### Task 3.4: Install PostgreSQL

- [ ] Install PostgreSQL 15+ (see Technical Guidance for your OS)
- [ ] Start PostgreSQL service
- [ ] Verify with `psql --version`

### Task 3.5: Create Database

```bash
# Connect to PostgreSQL (method depends on your setup)
psql postgres

# Run these SQL commands:
CREATE DATABASE relocation_advisor;
CREATE USER relocation_user WITH ENCRYPTED PASSWORD 'dev_password_123';
GRANT ALL PRIVILEGES ON DATABASE relocation_advisor TO relocation_user;
\q
```

- [ ] Database created
- [ ] User created
- [ ] Privileges granted

### Task 3.6: Test Database Connection

```bash
psql -h localhost -U relocation_user -d relocation_advisor -c "SELECT version();"
# Enter password when prompted
```

- [ ] Connection successful
- [ ] PostgreSQL version displayed

### Task 3.7: Create Environment Files

Create `.env.example` in project root:
```env
# Database
DATABASE_URL=postgresql://relocation_user:your_password@localhost:5432/relocation_advisor

# API
API_HOST=localhost
API_PORT=8000
DEBUG=true

# Frontend (for later)
REACT_APP_API_URL=http://localhost:8000
```

Create `.env` (with your actual password):
```env
DATABASE_URL=postgresql://relocation_user:dev_password_123@localhost:5432/relocation_advisor
API_HOST=localhost
API_PORT=8000
DEBUG=true
REACT_APP_API_URL=http://localhost:8000
```

- [ ] `.env.example` created
- [ ] `.env` created (with real values)
- [ ] Verified `.env` is in `.gitignore`

---

## Day 4: Remaining Structure and Documentation

### Task 4.1: Create Remaining Directories

```bash
# From project root
mkdir -p database/migrations
mkdir -p database/seeds
mkdir -p docs/architecture
mkdir -p docs/api
mkdir -p docs/data-dictionary
mkdir -p scripts

# Create placeholder files
touch database/migrations/.gitkeep
touch database/seeds/.gitkeep
touch docs/architecture/.gitkeep
touch docs/api/.gitkeep
touch docs/data-dictionary/.gitkeep
touch scripts/.gitkeep
```

- [ ] All directories created
- [ ] `.gitkeep` files added

### Task 4.2: Create Backend README

Create `backend/README.md`:
```markdown
# Relocation Advisor - Backend

Python FastAPI backend for the Relocation Advisor application.

## Setup

1. Create virtual environment:
   ```bash
   python -m venv venv
   source venv/bin/activate  # Mac/Linux
   .\venv\Scripts\activate   # Windows
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Run the server:
   ```bash
   uvicorn app.main:app --reload
   ```

4. Open http://localhost:8000/docs for API documentation

## Project Structure

- `app/` - Main application code
  - `models/` - Database models (SQLAlchemy)
  - `schemas/` - Request/Response schemas (Pydantic)
  - `routers/` - API route handlers
  - `services/` - Business logic
- `etl/` - Data ingestion scripts
- `tests/` - Test files
```

- [ ] Backend README created

### Task 4.3: Update Project README

Update the root `README.md`:
```markdown
# Relocation Advisor

A data-driven tool to help users find the best place to live based on their personal preferences.

## Overview

Relocation Advisor integrates data from multiple federal sources (Census, BLS, NOAA) to provide personalized location rankings based on:
- Cost of living
- Job market conditions
- Climate preferences
- Quality of life indicators

## Tech Stack

- **Backend:** Python, FastAPI, SQLAlchemy
- **Database:** PostgreSQL
- **Frontend:** React, TypeScript
- **Visualization:** Chart.js, Leaflet

## Project Structure

```
relocation-advisor/
├── backend/          # Python FastAPI application
├── frontend/         # React TypeScript application
├── database/         # Migrations and seed data
├── docs/             # Project documentation
└── scripts/          # Utility scripts
```

## Getting Started

See individual README files in `backend/` and `frontend/` directories.

## Development Status

This project is under active development as a learning exercise.
```

- [ ] Project README updated

### Task 4.4: Create Development Journal

Create `JOURNAL.md` in project root:
```markdown
# Development Journal

## Sprint 0: Onboarding & Environment Setup

**Date:** [Your date]
**Status:** Complete

### What I Built
[Describe what you set up - repository, environments, database, etc.]

### What I Learned
[New concepts, tools, or skills you picked up]

### What Was Challenging
[Things that were hard or took longer than expected]

### What I'd Do Differently
[Hindsight reflections]

### Questions I Have
[Things you want to learn more about]

---

*Journal entries for future sprints will go below this line*
```

- [ ] JOURNAL.md created with Sprint 0 entry

### Task 4.5: Final Commits

```bash
git add .
git commit -m "feat: complete project structure and documentation"
git push

# Verify on GitHub
```

- [ ] All changes committed
- [ ] Pushed to GitHub
- [ ] Verified structure on GitHub

---

## Verification Checklist

Before marking Sprint 0 complete, verify:

### Git & GitHub
- [ ] Repository exists on GitHub
- [ ] At least 3 meaningful commits in history
- [ ] `.gitignore` is working (no `node_modules`, `venv`, `.env`)

### Python
- [ ] `python --version` returns 3.11+
- [ ] Can activate virtual environment
- [ ] `pip list` shows installed packages
- [ ] FastAPI server runs with `uvicorn app.main:app --reload`
- [ ] http://localhost:8000 returns JSON
- [ ] http://localhost:8000/docs shows Swagger UI

### Node.js
- [ ] `node --version` returns 18+
- [ ] `npm --version` returns valid version

### PostgreSQL
- [ ] `psql --version` returns 15+
- [ ] Can connect to `relocation_advisor` database
- [ ] User `relocation_user` has proper permissions

### Project Structure
- [ ] All directories from Technical Guidance exist
- [ ] All placeholder files exist
- [ ] READMEs are in place

### Documentation
- [ ] JOURNAL.md has Sprint 0 entry
- [ ] .env.example exists (with no real secrets)
- [ ] .env exists locally (with real values)

---

## Sprint 0 Complete!

If all checkboxes are checked, you've successfully completed Sprint 0.

Take a moment to:
1. Review your commit history on GitHub
2. Make sure your journal entry is thorough
3. Pat yourself on the back

Then proceed to `sprint-1-database-design/` when ready.
