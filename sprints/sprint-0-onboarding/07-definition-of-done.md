# Definition of Done - Sprint 0

**Sprint:** 0 - Onboarding & Environment Setup

---

## What "Done" Means

A sprint is complete when:
1. All user stories meet their acceptance criteria
2. Code is committed with meaningful messages
3. Documentation is updated
4. You can demonstrate the deliverables
5. Journal entry is written

---

## Sprint 0 Checklist

### Repository & Git ✓
- [ ] GitHub repository `relocation-advisor` exists
- [ ] Repository is cloned locally
- [ ] `.gitignore` is properly configured
- [ ] At least 5 commits in history with meaningful messages
- [ ] No secrets or sensitive data in commit history

### Python Environment ✓
- [ ] Python 3.11+ installed and accessible
- [ ] Virtual environment created in `backend/venv`
- [ ] Can activate virtual environment
- [ ] FastAPI and dependencies installed
- [ ] `requirements.txt` reflects installed packages
- [ ] `uvicorn app.main:app --reload` starts server
- [ ] http://localhost:8000 returns JSON response
- [ ] http://localhost:8000/docs shows Swagger UI

### Node.js Environment ✓
- [ ] Node.js 18+ installed
- [ ] npm working
- [ ] Frontend folder structure created

### PostgreSQL ✓
- [ ] PostgreSQL 15+ installed and running
- [ ] Database `relocation_advisor` created
- [ ] User `relocation_user` created with password
- [ ] Can connect: `psql -h localhost -U relocation_user -d relocation_advisor`

### Project Structure ✓
- [ ] All directories from Technical Guidance exist
- [ ] `__init__.py` files in all Python packages
- [ ] `.gitkeep` files in empty directories
- [ ] Structure matches specification exactly

### Configuration ✓
- [ ] `.env.example` in project root (committed)
- [ ] `.env` in project root (NOT committed)
- [ ] `.env` contains valid database URL
- [ ] VS Code (or IDE) configured with extensions

### Documentation ✓
- [ ] Root `README.md` has project overview
- [ ] `backend/README.md` has setup instructions
- [ ] `frontend/README.md` exists with placeholder
- [ ] `JOURNAL.md` has Sprint 0 entry

---

## Verification Script

Run this to verify your setup (from project root):

```bash
#!/bin/bash
echo "=== Checking Git ==="
git --version
git remote -v
echo "Commits: $(git rev-list --count HEAD)"

echo -e "\n=== Checking Python ==="
python --version
cd backend
source venv/bin/activate 2>/dev/null || source venv/Scripts/activate 2>/dev/null
pip --version
echo "Packages: $(pip list | wc -l)"

echo -e "\n=== Checking Node ==="
node --version
npm --version

echo -e "\n=== Checking PostgreSQL ==="
psql --version
psql -h localhost -U relocation_user -d relocation_advisor -c "SELECT 'Database OK';" 2>/dev/null

echo -e "\n=== Checking Files ==="
cd ..
for f in README.md JOURNAL.md .gitignore .env.example; do
  [ -f "$f" ] && echo "✓ $f exists" || echo "✗ $f missing"
done

for d in backend frontend database docs scripts; do
  [ -d "$d" ] && echo "✓ $d/ exists" || echo "✗ $d/ missing"
done

echo -e "\n=== Setup Complete ==="
```

---

## Demo Requirements

You should be able to demonstrate:

1. **Git workflow**
   - Show commit history: `git log --oneline`
   - Show remote: `git remote -v`
   - Verify on GitHub

2. **Python environment**
   - Activate venv
   - Show installed packages
   - Start FastAPI server
   - Open Swagger docs in browser

3. **Database**
   - Connect via psql
   - Show database exists
   - Exit cleanly

4. **Project structure**
   - Navigate through folders
   - Explain what goes where

---

## Common Issues at Sprint End

### "I can't start the FastAPI server"
- Is venv activated?
- Are you in the `backend` directory?
- Did you install requirements?

### "PostgreSQL connection refused"
- Is PostgreSQL service running?
- Did you use the right password?
- Is the database created?

### "Git says I have uncommitted changes"
- Check `git status`
- Are they files that should be in `.gitignore`?
- Commit or stash as appropriate

### "My .env file is showing on GitHub"
- Remove it from Git: `git rm --cached .env`
- Add to `.gitignore` if not already there
- Commit the removal
- Change any passwords that were exposed!

---

## Sign-Off

Before proceeding to Sprint 1, confirm:

- [ ] I have completed all checklist items above
- [ ] I have written a thorough journal entry
- [ ] I can explain what I set up and why
- [ ] I know where to find help if I get stuck

---

## Tech Lead Review Simulation

In a real job, your tech lead would review your setup. Ask yourself:

1. If Sarah looked at my GitHub repo, would it look professional?
2. Is my commit history clear and meaningful?
3. Could another developer clone my repo and get it running?
4. Have I documented my setup choices?

If yes to all, proceed to Sprint 1!

---

*Congratulations on completing Sprint 0! Go to `../sprint-1-database-design/` to continue.*
