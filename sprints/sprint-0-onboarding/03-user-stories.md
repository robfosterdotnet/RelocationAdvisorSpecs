# User Stories - Sprint 0

**Sprint:** 0 - Onboarding & Environment Setup
**Assigned To:** Andrew [Your Last Name]

---

## How to Read User Stories

User stories follow this format:
> **As a** [type of user]
> **I want** [goal]
> **So that** [benefit]

For Sprint 0, you're the user - a developer who needs a working environment.

---

## Story S0-1: Repository Creation

**As a** developer
**I want** a properly initialized Git repository on GitHub
**So that** I can version control my code and collaborate with the team

### Acceptance Criteria
- [ ] Repository is created on GitHub
- [ ] Repository is named `relocation-advisor`
- [ ] Repository has a README.md with basic project info
- [ ] Repository has a proper `.gitignore` file
- [ ] Local clone is set up with remote pointing to GitHub
- [ ] At least one commit has been pushed to `main` branch

### Story Points: 2

### Notes from Tech Lead (Sarah)
> "Use the GitHub web interface to create the repo, then clone it locally. Don't initialize with a README from GitHub - we'll create our own with the proper structure. Make sure your Git identity is configured with your real name and email."

---

## Story S0-2: Python Environment Setup

**As a** developer
**I want** a working Python environment with virtual environment support
**So that** I can develop the backend API in an isolated environment

### Acceptance Criteria
- [ ] Python 3.11+ is installed and accessible from command line
- [ ] `python --version` returns 3.11 or higher
- [ ] Virtual environment can be created with `python -m venv venv`
- [ ] Virtual environment can be activated
- [ ] `pip` works within the virtual environment
- [ ] A `requirements.txt` file exists (can be empty initially)

### Story Points: 2

### Notes from Tech Lead (Sarah)
> "I recommend using `pyenv` on Mac/Linux for managing Python versions, but the system Python is fine if it's 3.11+. Always use virtual environments - never install packages globally. On Mac, do NOT use the system Python that comes with the OS."

---

## Story S0-3: Node.js Environment Setup

**As a** developer
**I want** a working Node.js environment
**So that** I can develop the React frontend

### Acceptance Criteria
- [ ] Node.js 18+ is installed
- [ ] `node --version` returns 18 or higher
- [ ] `npm --version` returns a valid version
- [ ] Can create a new directory and run `npm init`
- [ ] Can install a package with `npm install`

### Story Points: 1

### Notes from Tech Lead (Sarah)
> "I recommend using `nvm` (Node Version Manager) so you can switch versions if needed. The LTS version is fine. Don't worry about yarn vs npm - we'll use npm for this project."

---

## Story S0-4: PostgreSQL Installation

**As a** developer
**I want** PostgreSQL installed and running locally
**So that** I have a database for the application

### Acceptance Criteria
- [ ] PostgreSQL 15+ is installed
- [ ] PostgreSQL service is running
- [ ] Can connect using `psql` command-line tool
- [ ] Can create a database named `relocation_advisor`
- [ ] Can connect to the database
- [ ] Know the connection credentials (host, port, user, password)

### Story Points: 3

### Notes from Tech Lead (Sarah)
> "PostgreSQL can be tricky to install depending on your OS. On Mac, I recommend Postgres.app or Homebrew. On Windows, use the official installer. On Linux, use your package manager. The default port is 5432. Document your credentials somewhere secure - you'll need them later."

---

## Story S0-5: IDE Setup

**As a** developer
**I want** a properly configured IDE
**So that** I can write code efficiently with proper tooling support

### Acceptance Criteria
- [ ] VS Code (or preferred IDE) is installed
- [ ] Python extension is installed and working
- [ ] ESLint extension is installed
- [ ] Prettier extension is installed
- [ ] Can open the project folder
- [ ] Syntax highlighting works for Python and TypeScript
- [ ] GitLens extension installed (optional but recommended)

### Story Points: 1

### Notes from Tech Lead (Sarah)
> "VS Code is the team standard, but if you're more comfortable with PyCharm or WebStorm, that's fine too. The key is having good Python and JavaScript/TypeScript support. Don't spend too much time customizing themes and settings - you can do that later."

---

## Story S0-6: Project Structure Creation

**As a** developer
**I want** a well-organized project folder structure
**So that** all code has a logical home and the team can navigate easily

### Acceptance Criteria
- [ ] Folder structure matches the specification in Technical Guidance
- [ ] Each major folder has a `.gitkeep` or placeholder file
- [ ] Structure is committed to Git
- [ ] No unnecessary files are tracked

### Story Points: 2

### Notes from Tech Lead (Sarah)
> "The folder structure is documented in the technical guidance. Don't deviate from it without discussing with me first. Empty folders don't get tracked by Git, so we use `.gitkeep` files as placeholders."

---

## Story S0-7: Development Journal Entry

**As a** developer
**I want** to document my Sprint 0 experience
**So that** I can reflect on learning and have material for interviews

### Acceptance Criteria
- [ ] `JOURNAL.md` file exists in project root
- [ ] Sprint 0 entry includes: what was done, what was learned, what was challenging
- [ ] Entry is written in complete sentences (not just bullet points)
- [ ] Entry is committed to Git

### Story Points: 1

### Notes from Tech Lead (Sarah)
> "This is for you more than for the team. Write honestly about what was hard and what you learned. This will be valuable when you're interviewing and someone asks about challenges you've overcome."

---

## Sprint Velocity Target

**Total Story Points:** 12

This is a baseline sprint to establish your velocity. Track how long these stories take you - it'll help with planning future sprints.

---

## Definition of Ready

A story is ready to work on when:
- You've read and understood the acceptance criteria
- You've read any tech lead notes
- You have no blocking questions
- You have the necessary access and tools

## Definition of Done

A story is done when:
- All acceptance criteria are met
- Changes are committed with meaningful messages
- No new linting errors or warnings introduced
- You could explain what you did to another developer

---

*Next: Read `04-technical-guidance.md`*
