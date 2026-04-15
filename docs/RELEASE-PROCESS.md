# Release Process

This document defines how ReadyToMeow releases software, from versioning strategy to deployment and rollback.

## Versioning

ReadyToMeow uses [Semantic Versioning](https://semver.org/):

```
MAJOR.MINOR.PATCH
```

| Type | When to bump | Example |
|------|--------------|---------|
| **MAJOR** | Breaking changes to APIs, data models, or user-facing behavior | `1.0.0` → `2.0.0` |
| **MINOR** | New functionality (backward-compatible) | `1.0.0` → `1.1.0` |
| **PATCH** | Bug fixes, security patches (backward-compatible) | `1.0.0` → `1.0.1` |

Each repository (`meow-sheet-shop`, `meow-sheet-shop-be`) maintains its own version. The monorepo version reflects the highest-versioned release across all components.

## Changelog

Every release updates `CHANGELOG.md` using the [Keep a Changelog](https://keepachangelog.com/) format:

```markdown
# Changelog

## [X.Y.Z] - YYYY-MM-DD

### Added
- New feature description

### Changed
- Change description

### Fixed
- Fix description

### Removed
- Removed feature description
```

### Changelog Rules

- **User-visible only** — document what users experience, not internal refactors
- **Be specific** — "Fixed login timeout on Safari" not "Fixed bug"
- **One section per change type** — group related changes

## Release Workflow

### Prerequisites

- All PRs for this release are merged to `main`
- CI passes on `main`
- No critical bugs open for this release

### Steps

1. **Review commits since last release**

   ```bash
   git log --oneline vX.Y.Z..main
   ```

   Identify changes that warrant version bumps.

2. **Determine version bump**

   - Breaking changes → MAJOR
   - New features → MINOR
   - Bug fixes → PATCH

3. **Update CHANGELOG.md**

   Add a new entry at the top with today's date and relevant changes.

4. **Update version in package manifests**

   Update `version` in each repository's `package.json`:
   - `meow-sheet-shop/package.json`
   - `meow-sheet-shop-be/package.json`

5. **Commit release**

   ```bash
   git add CHANGELOG.md meow-sheet-shop/package.json meow-sheet-shop-be/package.json
   git commit -m "chore: release vX.Y.Z"
   ```

6. **Tag the release**

   ```bash
   git tag -a vX.Y.Z -m "Release vX.Y.Z"
   ```

7. **Push with tags**

   ```bash
   git push origin main --tags
   ```

8. **Create GitHub Release** (when CI is configured)

   ```bash
   gh release create vX.Y.Z \
     --title "vX.Y.Z" \
     --notes "$(cat CHANGELOG.md | head -20)"
   ```

## Repository-Specific Releases

Each repository (`meow-sheet-shop`, `meow-sheet-shop-be`) follows the same release process independently. Coordinate releases when changes span both repositories by:

1. Releasing the backend first (`meow-sheet-shop-be`)
2. Releasing the frontend second (`meow-sheet-shop`)

This ensures the frontend is always compatible with the currently deployed backend.

## Rollback

If a release causes issues:

### Revert the release commit

```bash
git revert <release-commit-sha>
git push origin main
```

### Deploy previous tag

```bash
git checkout vX.Y.Z
# redeploy previous version
```

### Issue a patch

If the fix is quick, patch it:

```bash
git checkout main
git cherry-pick <fix-commit>
# follow release workflow for a PATCH bump
```

## CI/CD Release Automation

When CI is configured, releases trigger automatically on tagged commits. Until then:

- Releases are manual, following the steps above
- CI must pass on `main` before any release
- Tag push triggers the release workflow (if configured)

## Current State

The repositories `meow-sheet-shop` and `meow-sheet-shop-be` have not yet been added to this monorepo. This release process will apply once the repositories are integrated.

For monorepo-level documentation releases (this repo only), follow the same workflow with version bumps reflecting documentation/infra changes only.

## Hotfix Process

For urgent production fixes:

1. Create a branch from `main`: `hotfix-<brief-description>`
2. Make the minimal fix
3. Follow the PR process with expedited review
4. Merge and release immediately (PATCH bump)
5. Do not skip CI

---
_Documented during RTM-7. Update as release infrastructure is established._