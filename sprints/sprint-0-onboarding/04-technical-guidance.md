# Technical Guidance - Sprint 0

**From:** Sarah Chen (Tech Lead)
**To:** Andrew [Your Last Name]
**Subject:** Environment Setup and Project Structure

---

## Overview

This document provides the technical specifications you need to complete Sprint 0. Follow these instructions carefully. If something doesn't work, troubleshoot before asking for help.

---

## Project Folder Structure

Create exactly this structure in your repository:

```
relocation-advisor/
├── README.md                 # Project overview (create during setup)
├── JOURNAL.md               # Your development journal
├── .gitignore               # Git ignore rules
├── .env.example             # Example environment variables (no secrets!)
│
├── backend/                 # Python FastAPI application
│   ├── app/
│   │   ├── __init__.py
│   │   ├── main.py          # FastAPI entry point
│   │   ├── config.py        # Configuration management
│   │   ├── database.py      # Database connection
│   │   ├── models/          # SQLAlchemy models
│   │   │   └── __init__.py
│   │   ├── schemas/         # Pydantic schemas
│   │   │   └── __init__.py
│   │   ├── routers/         # API route handlers
│   │   │   └── __init__.py
│   │   └── services/        # Business logic
│   │       └── __init__.py
│   ├── etl/                 # Data ingestion scripts
│   │   └── __init__.py
│   ├── tests/               # Backend tests
│   │   └── __init__.py
│   ├── requirements.txt     # Python dependencies
│   └── README.md            # Backend-specific docs
│
├── frontend/                # React TypeScript application
│   ├── public/
│   ├── src/
│   │   ├── components/      # Reusable React components
│   │   ├── pages/           # Page-level components
│   │   ├── services/        # API client code
│   │   ├── hooks/           # Custom React hooks
│   │   ├── types/           # TypeScript type definitions
│   │   ├── utils/           # Helper functions
│   │   ├── App.tsx
│   │   └── index.tsx
│   ├── package.json
│   └── README.md            # Frontend-specific docs
│
├── database/                # Database-related files
│   ├── migrations/          # Database migration scripts
│   └── seeds/               # Sample/test data
│
├── docs/                    # Project documentation
│   ├── architecture/        # Architecture diagrams/docs
│   ├── api/                 # API documentation
│   └── data-dictionary/     # Data definitions
│
└── scripts/                 # Utility scripts
    └── setup.sh             # Project setup script
```

---

## Git Configuration

### Initial Setup

```bash
# Configure your identity (use your real name and email)
git config --global user.name "Your Full Name"
git config --global user.email "your.email@example.com"

# Recommended settings
git config --global init.defaultBranch main
git config --global pull.rebase false
```

### .gitignore File

Create a `.gitignore` in your project root with:

```gitignore
# Python
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
venv/
env/
.env
*.egg-info/
dist/
build/

# Node
node_modules/
npm-debug.log
.npm
.eslintcache

# IDE
.vscode/settings.json
.idea/
*.swp
*.swo
.DS_Store

# Environment
.env
.env.local
*.local

# Database
*.db
*.sqlite

# Testing
.coverage
htmlcov/
.pytest_cache/

# Build
*.log
```

---

## Python Environment

### Installation (Mac with Homebrew)

```bash
# Install pyenv (Python version manager)
brew install pyenv

# Add to your shell profile (~/.zshrc or ~/.bash_profile)
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(pyenv init -)"' >> ~/.zshrc

# Reload shell
source ~/.zshrc

# Install Python 3.11
pyenv install 3.11
pyenv global 3.11
```

### Installation (Windows)

Download and install from https://www.python.org/downloads/

Make sure to check "Add Python to PATH" during installation.

### Virtual Environment Setup

```bash
# Navigate to backend folder
cd relocation-advisor/backend

# Create virtual environment
python -m venv venv

# Activate (Mac/Linux)
source venv/bin/activate

# Activate (Windows)
.\venv\Scripts\activate

# Verify activation (should show venv path)
which python

# Install initial dependencies
pip install --upgrade pip
pip install fastapi uvicorn sqlalchemy psycopg2-binary python-dotenv

# Save dependencies
pip freeze > requirements.txt
```

---

## Node.js Environment

### Installation (Mac with Homebrew)

```bash
# Install nvm (Node Version Manager)
brew install nvm

# Create nvm directory
mkdir ~/.nvm

# Add to shell profile
echo 'export NVM_DIR="$HOME/.nvm"' >> ~/.zshrc
echo '[ -s "/opt/homebrew/opt/nvm/nvm.sh" ] && \. "/opt/homebrew/opt/nvm/nvm.sh"' >> ~/.zshrc

# Reload shell
source ~/.zshrc

# Install Node.js LTS
nvm install --lts
nvm use --lts
```

### Installation (Windows)

Download and install from https://nodejs.org/ (LTS version)

### Verify Installation

```bash
node --version  # Should be 18+
npm --version   # Should be 9+
```

---

## PostgreSQL Setup

### Installation (Mac with Homebrew)

```bash
# Install PostgreSQL
brew install postgresql@15

# Start the service
brew services start postgresql@15

# Add to PATH (add to ~/.zshrc)
echo 'export PATH="/opt/homebrew/opt/postgresql@15/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

### Installation (Mac with Postgres.app)

1. Download from https://postgresapp.com/
2. Move to Applications folder
3. Open and click "Initialize"
4. Add to PATH: Add `/Applications/Postgres.app/Contents/Versions/latest/bin` to your PATH

### Installation (Windows)

1. Download from https://www.postgresql.org/download/windows/
2. Run installer, follow prompts
3. Remember the password you set for the postgres user!

### Database Setup

```bash
# Connect to PostgreSQL
psql postgres

# In psql, create database and user
CREATE DATABASE relocation_advisor;
CREATE USER relocation_user WITH ENCRYPTED PASSWORD 'your_password_here';
GRANT ALL PRIVILEGES ON DATABASE relocation_advisor TO relocation_user;

# Exit psql
\q
```

### Test Connection

```bash
psql -h localhost -U relocation_user -d relocation_advisor
# Enter password when prompted
# You should see the psql prompt
\q
```

---

## Environment Variables

Create a `.env.example` file in the project root:

```env
# Database
DATABASE_URL=postgresql://relocation_user:your_password@localhost:5432/relocation_advisor

# API
API_HOST=localhost
API_PORT=8000
DEBUG=true

# Frontend
REACT_APP_API_URL=http://localhost:8000
```

Then create a `.env` file (which is gitignored) with your actual values.

**IMPORTANT:** Never commit `.env` files with real credentials!

---

## VS Code Extensions

Install these extensions (search in VS Code marketplace):

### Required
- **Python** (Microsoft) - Python language support
- **Pylance** (Microsoft) - Python type checking
- **ESLint** - JavaScript/TypeScript linting
- **Prettier** - Code formatting

### Recommended
- **GitLens** - Enhanced Git integration
- **Thunder Client** - API testing (like Postman)
- **PostgreSQL** (by Chris Kolkman) - Database viewer
- **Error Lens** - Inline error highlighting
- **Auto Rename Tag** - For HTML/JSX

### VS Code Settings

Add to your `.vscode/settings.json` (create in project):

```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "[python]": {
    "editor.defaultFormatter": "ms-python.python",
    "editor.formatOnSave": true
  },
  "python.linting.enabled": true,
  "python.linting.pylintEnabled": false,
  "python.linting.flake8Enabled": true,
  "typescript.preferences.importModuleSpecifier": "relative"
}
```

---

## Commit Message Format

Use this format for commit messages:

```
<type>: <short description>

[optional body]

[optional footer]
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Formatting, no code change
- `refactor`: Code restructuring
- `test`: Adding tests
- `chore`: Maintenance tasks

**Examples:**
```
feat: add user story templates
docs: update README with setup instructions
chore: configure Python virtual environment
```

---

## Verification Commands

Run these to verify your setup is complete:

```bash
# Git
git --version
git remote -v  # Should show your GitHub repo

# Python
python --version  # Should be 3.11+
pip --version

# Node
node --version  # Should be 18+
npm --version

# PostgreSQL
psql --version
psql -h localhost -U relocation_user -d relocation_advisor -c "SELECT 1;"
```

---

## Common Issues and Solutions

### "command not found: python"
- On Mac, try `python3` instead
- Check if Python is in your PATH
- Restart your terminal after installation

### PostgreSQL connection refused
- Make sure the PostgreSQL service is running
- Check if you're using the correct port (default 5432)
- Verify username and password

### Permission denied (Git)
- Make sure you've set up SSH keys or HTTPS credentials
- Check repository permissions on GitHub

### Node packages not found after install
- Make sure you're in the right directory
- Try deleting `node_modules` and `package-lock.json`, then reinstall

---

## Getting Help

1. **Read the error message** - It usually tells you what's wrong
2. **Google the error** - Copy the exact message
3. **Check Stack Overflow** - Someone has had this problem before
4. **Check official docs** - Sometimes they have troubleshooting sections
5. **Ask with context** - Explain what you tried, what you expected, what happened

---

*Next: Read `05-developer-tasks.md`*
