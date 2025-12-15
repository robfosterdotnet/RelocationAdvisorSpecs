# Learning Resources - Sprint 0

**Focus Areas:** Git, Python Environment, PostgreSQL Basics

---

## Git Fundamentals

### Must Read
- [Git Handbook (GitHub)](https://guides.github.com/introduction/git-handbook/) - 10 min read
- [Git Cheat Sheet (GitHub)](https://education.github.com/git-cheat-sheet-education.pdf) - Keep handy

### Should Understand
- [Understanding Git Branches](https://www.atlassian.com/git/tutorials/using-branches) - You'll use branches starting Sprint 1
- [Writing Good Commit Messages](https://cbea.ms/git-commit/) - Best practices

### Key Concepts
- What is a commit?
- What is a branch?
- What is a remote?
- Difference between `git add`, `git commit`, and `git push`
- What does `.gitignore` do?

### Practice Commands
```bash
git status          # Check current state
git log --oneline   # View commit history
git diff            # See uncommitted changes
git branch          # List branches
git remote -v       # View remote repositories
```

---

## Python Virtual Environments

### Must Read
- [Python venv Documentation](https://docs.python.org/3/library/venv.html) - Official docs
- [Real Python: Virtual Environments](https://realpython.com/python-virtual-environments-a-primer/) - Great tutorial

### Key Concepts
- Why use virtual environments?
- What is `pip`?
- What is `requirements.txt`?
- Difference between global and local package installation

### Commands to Know
```bash
python -m venv venv     # Create virtual environment
source venv/bin/activate  # Activate (Mac/Linux)
pip install package     # Install a package
pip freeze > requirements.txt  # Save dependencies
pip install -r requirements.txt  # Install from file
deactivate              # Exit virtual environment
```

---

## FastAPI Basics (Preview for Sprint 4)

### Light Reading
- [FastAPI First Steps](https://fastapi.tiangolo.com/tutorial/first-steps/) - Just skim for now
- You'll dive deep in Sprint 4

### Key Concepts to Preview
- What is an API?
- What is REST?
- What is a route/endpoint?
- What is JSON?

---

## PostgreSQL Basics

### Must Read
- [PostgreSQL Tutorial](https://www.postgresqltutorial.com/postgresql-getting-started/) - Getting started section only
- [PostgreSQL Cheat Sheet](https://www.postgresqltutorial.com/postgresql-cheat-sheet/) - Keep handy

### Key Concepts
- What is a relational database?
- What is SQL?
- What is a table? A row? A column?
- What is a primary key?
- What are privileges/permissions?

### Commands to Know
```sql
-- psql meta-commands
\l          -- List databases
\c dbname   -- Connect to database
\dt         -- List tables
\d table    -- Describe table
\q          -- Quit

-- Basic SQL (preview)
SELECT * FROM table;
CREATE TABLE ...;
INSERT INTO ...;
```

---

## Terminal/Command Line

### If You're Not Comfortable
- [Command Line Crash Course](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Understanding_client-side_tools/Command_line) - MDN tutorial

### Essential Commands
```bash
pwd         # Print working directory
ls          # List files
cd          # Change directory
mkdir       # Make directory
touch       # Create empty file
rm          # Remove file (careful!)
cat         # Display file contents
which       # Find where a command lives
```

---

## VS Code

### Getting Started
- [VS Code Getting Started](https://code.visualstudio.com/docs/getstarted/introvideos) - Video tutorials

### Keyboard Shortcuts to Learn
| Action | Mac | Windows |
|--------|-----|---------|
| Command Palette | Cmd+Shift+P | Ctrl+Shift+P |
| Quick File Open | Cmd+P | Ctrl+P |
| Find in Files | Cmd+Shift+F | Ctrl+Shift+F |
| Toggle Terminal | Ctrl+` | Ctrl+` |
| Go to Definition | Cmd+Click | Ctrl+Click |

---

## Environment Variables

### Must Understand
- [12 Factor App: Config](https://12factor.net/config) - Why we use env variables
- Never commit secrets to Git

### Key Concepts
- What is an environment variable?
- What is `.env`?
- Why do we have `.env.example`?

---

## Debugging & Problem Solving

### Approach
1. **Read the error message** - Carefully, word by word
2. **Google the exact error** - Copy/paste the message
3. **Check Stack Overflow** - Look for accepted answers
4. **Read official docs** - Troubleshooting sections
5. **Isolate the problem** - What's the smallest reproducible case?

### Common Beginner Mistakes
- Not reading error messages carefully
- Trying to fix too many things at once
- Not checking if services are running
- Forgetting to activate virtual environment
- Typos in commands

---

## Optional: Deeper Dives

If you finish early and want to learn more:

### Git
- [Pro Git Book](https://git-scm.com/book/en/v2) - Free, comprehensive

### Python
- [Python Tutorial](https://docs.python.org/3/tutorial/) - Official tutorial

### SQL
- [SQLBolt](https://sqlbolt.com/) - Interactive SQL tutorial

### Web Development Concepts
- [MDN Web Docs](https://developer.mozilla.org/en-US/) - Excellent reference

---

## Study Time Recommendations

| Topic | Time | Priority |
|-------|------|----------|
| Git basics | 1-2 hours | High |
| Virtual environments | 30 min | High |
| PostgreSQL basics | 1 hour | Medium |
| Terminal commands | 30 min | Medium |
| VS Code shortcuts | 15 min | Low |

---

## Knowledge Check

Before moving on, make sure you can answer:

1. What's the difference between `git add` and `git commit`?
2. Why do we use virtual environments in Python?
3. What is the purpose of `requirements.txt`?
4. How do you connect to a PostgreSQL database from the command line?
5. Why should you never commit `.env` files?
6. What command starts a FastAPI server?

If you can't answer these, go back and review the relevant sections.

---

*Next: Read `07-definition-of-done.md`*
