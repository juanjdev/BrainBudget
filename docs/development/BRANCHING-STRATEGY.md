# Branching Strategy

## Overview

BrainBudget uses a **simplified GitHub Flow** strategy optimized for a single developer workflow with professional practices.

## Branch Structure

```
main              → Production-ready code (protected)
├── develop       → Integration branch (default branch)
└── feature/*     → Individual features and tasks
    ├── feature/auth-setup
    ├── feature/prisma-config
    └── feature/tailwind-setup
```

---

## Branch Types

### **`main`** - Production Branch
- **Purpose**: Production-ready, stable code only
- **Protected**: Yes
- **Merges from**: `develop` only (via PR)
- **Deployment**: Auto-deploy to production
- **Rules**:
  - No direct commits
  - Requires PR review (self-review for solo dev)
  - All tests must pass

### **`develop`** - Integration Branch
- **Purpose**: Main development branch, integration point
- **Default branch**: Yes (for new clones)
- **Merges from**: `feature/*` branches
- **Merges to**: `main` (when stable)
- **Rules**:
  - Direct commits allowed for small fixes
  - Prefer feature branches for significant work

### **`feature/*`** - Feature Branches
- **Purpose**: Individual features, tasks, or experiments
- **Naming convention**: `feature/short-description`
- **Created from**: `develop`
- **Merges to**: `develop` (via PR)
- **Lifespan**: Short-lived (delete after merge)

---

## Workflow

### **Starting New Work**

```bash
# 1. Update develop
git checkout develop
git pull origin develop

# 2. Create feature branch
git checkout -b feature/task-name

# 3. Work on feature
git add .
git commit -m "feat: implement task"

# 4. Push feature branch
git push -u origin feature/task-name

# 5. Create PR: feature/task-name → develop
# 6. Review, test, merge PR
# 7. Delete feature branch
git branch -d feature/task-name
```

### **Preparing Release**

```bash
# 1. Ensure develop is stable
git checkout develop
npm run test
npm run build

# 2. Create PR: develop → main
# 3. Review changes carefully
# 4. Merge to main
# 5. Tag release
git checkout main
git tag -a v0.1.0 -m "Release v0.1.0"
git push origin v0.1.0
```

---

## Commit Message Convention

Follow **Conventional Commits** specification:

```
<type>(<scope>): <subject>

<body>

<footer>
```

### **Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting)
- `refactor`: Code refactoring
- `test`: Adding or updating tests
- `chore`: Maintenance tasks (dependencies, configs)
- `perf`: Performance improvements
- `ci`: CI/CD changes

### **Examples:**

```bash
feat(auth): implement JWT authentication
fix(api): resolve CORS issue in production
docs(readme): update installation instructions
chore(deps): upgrade Next.js to v16
test(user): add integration tests for user service
```

---

## Branch Naming Conventions

### **Feature Branches**
```
feature/user-authentication
feature/expense-crud
feature/receipt-ocr
feature/budget-alerts
```

### **Bugfix Branches**
```
fix/login-redirect-issue
fix/api-cors-error
```

### **Documentation Branches**
```
docs/setup-guide
docs/api-documentation
```

### **Chore/Maintenance Branches**
```
chore/upgrade-dependencies
chore/cleanup-unused-code
```

---

## Pull Request Guidelines

### **PR Title Format**
Same as commit convention:
```
feat(auth): implement JWT authentication with refresh tokens
```

### **PR Description Template**

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Feature
- [ ] Bug Fix
- [ ] Documentation
- [ ] Chore/Maintenance

## Related Issue/Task
Link to Notion task or GitHub issue

## Changes Made
- Change 1
- Change 2
- Change 3

## Testing
- [ ] Unit tests pass
- [ ] E2E tests pass
- [ ] Manual testing completed

## Screenshots (if applicable)
[Add screenshots]

## Checklist
- [ ] Code follows project style guidelines
- [ ] Self-review completed
- [ ] Documentation updated
- [ ] No console errors or warnings
```

---

## Protected Branches

### **`main` Protection Rules**
- ✅ Require pull request reviews before merging
- ✅ Require status checks to pass (CI/CD)
- ✅ Require branches to be up to date before merging
- ✅ Do not allow bypassing the above settings

### **`develop` Protection Rules** (Optional for solo dev)
- ⚪ Allow direct commits for small fixes
- ✅ Recommend PRs for significant features

---

## Hotfix Process

For critical production bugs:

```bash
# 1. Create hotfix branch from main
git checkout main
git checkout -b hotfix/critical-bug-name

# 2. Fix the issue
git add .
git commit -m "fix: resolve critical bug"

# 3. Create PR to main
# Review and merge

# 4. Merge main back to develop
git checkout develop
git merge main
git push origin develop
```

---

## Release Workflow

### **Semantic Versioning**

Follow **SemVer**: `MAJOR.MINOR.PATCH`

- **MAJOR**: Breaking changes (v1.0.0 → v2.0.0)
- **MINOR**: New features, backwards compatible (v1.0.0 → v1.1.0)
- **PATCH**: Bug fixes (v1.0.0 → v1.0.1)

### **Release Process**

```bash
# 1. Create release branch
git checkout develop
git checkout -b release/v1.0.0

# 2. Update version numbers
# - package.json
# - CHANGELOG.md

# 3. Final testing
npm run test
npm run build

# 4. Create PR to main
# Merge when ready

# 5. Tag release
git checkout main
git tag -a v1.0.0 -m "Release v1.0.0 - Initial release"
git push origin v1.0.0

# 6. Merge back to develop
git checkout develop
git merge main
```

---

## Best Practices

### **DO:**
✅ Keep feature branches small and focused
✅ Commit often with meaningful messages
✅ Pull from develop frequently to stay updated
✅ Delete merged feature branches
✅ Write descriptive PR descriptions
✅ Link PRs to Notion tasks/tickets
✅ Run tests before creating PRs

### **DON'T:**
❌ Commit directly to main
❌ Create long-lived feature branches
❌ Use generic commit messages ("fix", "update")
❌ Leave merged branches undeleted
❌ Skip PR descriptions
❌ Push broken code to develop

---

## Emergency Procedures

### **Accidentally Committed to Main**

```bash
# 1. Create branch from current state
git branch feature/accidental-commit

# 2. Reset main to previous state
git checkout main
git reset --hard origin/main

# 3. Push feature branch
git checkout feature/accidental-commit
git push -u origin feature/accidental-commit

# 4. Create PR normally
```

### **Need to Undo Last Commit**

```bash
# Undo commit but keep changes
git reset --soft HEAD~1

# Undo commit and discard changes
git reset --hard HEAD~1
```

---

## Branch Cleanup

### **Delete Local Merged Branches**

```bash
# List merged branches
git branch --merged

# Delete specific branch
git branch -d feature/branch-name

# Delete all merged branches except main/develop
git branch --merged | grep -v "\*\|main\|develop" | xargs -n 1 git branch -d
```

### **Delete Remote Branches**

```bash
# Delete specific remote branch
git push origin --delete feature/branch-name

# Prune deleted remote branches locally
git fetch --prune
```

---

## References

- [GitHub Flow](https://docs.github.com/en/get-started/quickstart/github-flow)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [Semantic Versioning](https://semver.org/)

---

**Last Updated**: 2025-10-30
**Status**: Active
