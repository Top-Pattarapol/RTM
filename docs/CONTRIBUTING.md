# Contributing to ReadyToMeow

Welcome! This guide gets you from zero to your first merged PR. Read it fully before making changes.

---

## Project Overview

ReadyToMeow is a Thai cat food e-commerce platform with two repositories:

| Repository | Role | Stack |
|------------|------|-------|
| `frontend` | Customer and admin UI | React 18, Vite 7, TypeScript, TailwindCSS, Radix UI |
| `backend` | REST API, auth, payments | Express 4, Node.js (ES Modules), Google Sheets API |

The project uses Google Sheets as its database and Firebase for file storage and authentication.

**Key docs to bookmark:**
- [ARCHITECTURE.md](./ARCHITECTURE.md) — System design and component relationships
- [TECH-STACK.md](./TECH-STACK.md) — Technology choices and rationale
- [git-workflow.md](/RTM/docs/git-workflow) — Branch constraints and commit rules
- [pr-conventions.md](/RTM/docs/pr-conventions) — PR lifecycle and review roles

---

## Prerequisites

Before contributing, ensure you have:

- **Git** configured on your machine
- **Node.js 18+** installed (backend uses ES Modules)
- **npm 9+** installed
- Access to the repository (ask the team via Paperclip)
- Basic familiarity with REST APIs, React, and Express

---

## Setting Up Your Environment

### 1. Clone the Repository

```bash
git clone <repository-url>
cd RTM
```

The project is structured as a monorepo with `frontend/` and `backend/` as git submodules.

### 2. Set Up the Backend

```bash
cd projects/RTM/backend
npm install
cp .env.example .env
```

Edit `.env` with your credentials. Required variables:

| Variable | Purpose |
|----------|---------|
| `PORT` | Server port (default: 3001) |
| `GOOGLE_SHEETS_ID` | Google Sheets spreadsheet ID |
| `GOOGLE_SERVICE_ACCOUNT_EMAIL` | Service account for Sheets access |
| `GOOGLE_PRIVATE_KEY` | Service account private key |
| `FIREBASE_PROJECT_ID` | Firebase project ID |
| `FIREBASE_CLIENT_EMAIL` | Firebase service account email |
| `FIREBASE_PRIVATE_KEY` | Firebase private key |
| `FIREBASE_STORAGE_BUCKET` | Firebase storage bucket |
| `ACCESS_CODE_SECRET` | JWT signing secret (min 32 chars) |
| `ADMIN_TOKEN` | Admin authentication token |

Ask the team for development credentials if you don't have access to these.

Start the development server:

```bash
npm run dev
```

Verify it's running: `GET http://localhost:3001/health`

### 3. Set Up the Frontend

```bash
cd projects/RTM/frontend
npm install
npm run dev
```

The frontend runs on `http://localhost:5173` by default.

### 4. Verify the Setup

- Frontend loads at `http://localhost:5173`
- Backend health check: `http://localhost:3001/health`
- Swagger docs: `http://localhost:3001/api-docs`

---

## Branching and Commit Conventions

**All development work MUST be on `ai/` prefixed branches.** This is a hard constraint.

### Branch Naming

```
ai/<issue-number>/<short-description>
```

Examples:
- `ai-16/contributor-onboarding`
- `ai-42/fix-cart-quantity`
- `ai-88/add-product-search`

### Commit Messages

Use [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>: <short description>
```

**Types:** `feat`, `fix`, `docs`, `chore`, `refactor`, `test`, `perf`

**Rules:**
- Lowercase after colon
- No period at end
- Under 72 characters
- Reference issue ID in commit body when applicable

**Examples:**
- `feat: add user registration endpoint`
- `fix: resolve cart not updating on quantity change`
- `docs: update CONTRIBUTING with new setup steps`

---

## Making Changes

1. **Create your branch** from `ai/main`:

   ```bash
   git checkout ai/main
   git pull ai/main
   git checkout -b ai-<issue-number>/<description>
   ```

2. **Make your changes.** Keep changes focused and atomic.

3. **Test locally** before pushing:
   - Backend: `npm run lint`, verify health endpoint
   - Frontend: `npm run lint`, run dev server and verify UI

4. **Commit** with a conventional message.

5. **Push** to the remote:

   ```bash
   git push -u origin ai-<issue-number>/<description>
   ```

---

## Opening a Pull Request

1. Open a PR on GitHub.
2. Set the originating issue to `in_review`.
3. @-mention **Code Reviewer** and **Product Owner** on the issue with the PR link.
4. Fill in the PR body using this template:

   ```markdown
   ## What changed
   <Brief description of the changes>

   ## Why
   <Motivation and context>

   ## How to test
   <Steps to verify the changes>

   ## Related
   Closes [ISSUE-123]
   ```

---

## Review Process

Two-role review (both required for merge):

| Reviewer | Focus |
|----------|-------|
| **Code Reviewer** | Correctness, security, style, simplicity |
| **Product Owner** | Intent alignment, scope discipline, acceptance criteria |

**Merge requirements:**
- Both reviewers approve
- CI passes
- No force pushes

Engineer merges after both approvals. See [pr-conventions.md](/RTM/docs/pr-conventions) for full details.

---

## What Requires a PR

**Requires PR:** code logic, APIs, DB schema, agent configs, infrastructure

**Direct-to-main OK:** typos, comment-only changes, minor doc fixes (must still reference the issue)

---

## Project Structure

```
RTM/
├── backend/               # Express API server
│   ├── src/
│   │   ├── config/       # Swagger config
│   │   ├── lib/          # Google Sheets, Firebase, QR generation, etc.
│   │   ├── middleware/   # Auth middleware (admin.js, access.js)
│   │   ├── routes/       # API route handlers
│   │   ├── scheduler/    # Cron jobs (order expiration)
│   │   └── index.js      # Entry point
│   ├── tests/            # Integration tests
│   ├── docs/             # Backend-specific docs
│   └── package.json
├── frontend/             # React SPA
│   ├── src/
│   │   ├── components/   # UI components (ui/, admin/, catalog/, layout/)
│   │   ├── contexts/     # AuthContext
│   │   ├── hooks/        # Custom hooks (use-cart, use-mobile, etc.)
│   │   ├── lib/          # API clients, utilities
│   │   ├── pages/        # Route pages (customer/, admin/)
│   │   └── types/        # TypeScript types
│   └── package.json
└── docs/                 # Project documentation
    ├── ARCHITECTURE.md   # System architecture
    ├── TECH-STACK.md     # Technology choices
    ├── CONTRIBUTING.md   # This file
    └── ...
```

---

## Documentation Standards

Good documentation is part of good code. When you:

- **Add a feature** → document it in the relevant doc
- **Change an API** → update both Swagger docs and README
- **Fix a bug** → consider whether the docs could have prevented it

---

## Getting Help

- **Paperclip:** Tag the team on the issue
- **Swagger UI:** `http://localhost:3001/api-docs` for interactive API exploration
- **Existing docs:** Check `docs/` folder before asking

---

## Quick Reference

```bash
# Backend
cd projects/RTM/backend
npm run dev          # Start dev server
npm run lint         # Lint code
curl localhost:3001/health  # Verify running

# Frontend
cd projects/RTM/frontend
npm run dev          # Start dev server
npm run lint         # Lint code
npm run build        # Production build
```
