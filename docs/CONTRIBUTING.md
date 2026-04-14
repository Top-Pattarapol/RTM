# Contributing to ReadyToMeow

This guide covers how to set up your development environment, make changes, and get them merged.

## Prerequisites

Before contributing, ensure you have:
- Access to the `meow-sheet-shop` and `meow-sheet-shop-be` repositories
- Git configured on your machine
- Understanding of the git workflow (see [git-workflow.md](/RTM/docs/git-workflow))

## Setting Up

### 1. Clone the repositories

```bash
git clone <meow-sheet-shop-url>
git clone <meow-sheet-shop-be-url>
```

### 2. Install dependencies

Follow the setup instructions in each repository's README.

### 3. Verify the setup

Run any available tests or validation scripts to confirm your environment is working.

## Making Changes

### Branch Naming

```
rtm-<N>/<short-description>
```

Example: `rtm-13/create-initial-docs`

### Commit Messages

Use [Conventional Commits](https://www.conventionalcommits.org/) format:

```
<type>: <short description>
```

Types: `feat`, `fix`, `docs`, `chore`, `refactor`, `test`, `perf`

Examples:
- `feat: add user authentication endpoint`
- `fix: resolve null pointer in game loop`
- `docs: update README with setup instructions`

Rules:
- Lowercase after colon
- No period at end
- Under 72 characters
- Reference issue ID in commit body when applicable

### Opening a Pull Request

1. Push your branch to the remote
2. Open a PR on GitHub
3. Set the originating issue to `in_review`
4. @-mention **Code Reviewer** and **Product Owner** on the issue with the PR link

See [pr-conventions.md](/RTM/docs/pr-conventions) for the full review workflow.

## Review Process

Two-role review:
1. **Code Reviewer** — correctness, security, style, simplicity
2. **Product Owner** — intent alignment, scope discipline, acceptance criteria

Both must approve before merge. CI must pass. No force pushes.

See [pr-conventions.md](/RTM/docs/pr-conventions) for merge rules and review role details.

## What Requires a PR

**Requires PR**: code logic, APIs, DB schema, agent configs, infrastructure

**Direct-to-main OK**: typos, comment-only changes, minor doc fixes (must reference issue)

## Documentation

Good documentation is part of good code. When you:
- Add a new feature → document it
- Change an API → update the docs
- Fix a bug → consider whether docs needed updating

## Questions?

If you're unsure about anything, ask on the issue or reach out to the team via Paperclip.