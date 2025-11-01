# Notion Task Board Setup Guide

Complete guide to set up the BrainBudget task board in Notion.

---

## Board Structure

### Database Properties

Create a Notion database with these properties:

| Property | Type | Options |
|----------|------|---------|
| **Task** | Title | - |
| **Status** | Select | Backlog, To Do, In Progress, Done, Blocked |
| **Phase** | Select | Phase 0, Phase 1, Phase 2, ..., Phase 10 |
| **Priority** | Select | Critical, High, Medium, Low |
| **Type** | Multi-select | Feature, Bug, Docs, Config, Refactor, Test |
| **Estimate** | Number | Hours (1, 2, 3, 5, 8, 13) |
| **Assignee** | Person | Juan |
| **Due Date** | Date | - |
| **Branch** | Text | feature/task-name |
| **PR Link** | URL | - |
| **Tags** | Multi-select | frontend, backend, database, auth, etc. |
| **Notes** | Text | Additional context |

---

## Views

### 1. Kanban Board (Default View)
**Group by**: Status
**Sort by**: Priority (descending), then Due Date

Columns:
- 📋 Backlog
- 🎯 To Do
- 🔄 In Progress
- ✅ Done
- 🚫 Blocked

### 2. By Phase
**Group by**: Phase
**Filter**: Status != Done
**Sort by**: Priority

### 3. Current Sprint
**Filter**:
- Status = "To Do" OR "In Progress"
- Due Date is within 2 weeks

### 4. Completed
**Filter**: Status = Done
**Sort by**: Completed date (descending)

---

## Status Workflow

```
Backlog → To Do → In Progress → Done
              ↓           ↓
           Blocked ←──────┘
```

### Status Definitions

- **Backlog**: Ideas and future work, not prioritized yet
- **To Do**: Ready to start, prioritized for current/next sprint
- **In Progress**: Actively being worked on
- **Done**: Completed and merged
- **Blocked**: Cannot proceed due to dependency or issue

---

## Task Template

Use this template for new tasks:

```markdown
## Description
[Brief description of what needs to be done]

## Acceptance Criteria
- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

## Technical Notes
[Any technical details, links to docs, or implementation notes]

## Dependencies
- Depends on: [Link to other task]
- Blocks: [Link to tasks this blocks]

## Resources
- [Documentation link]
- [Reference implementation]
```

---

## Initial Backlog (First 30 Tasks)

Copy these tasks into your Notion board:

### Phase 0: Foundation & Setup (15 tasks)

#### Infrastructure
1. **Set up Docker Compose**
   - Type: Config
   - Priority: Critical
   - Estimate: 2h
   - Description: Create docker-compose.yml for PostgreSQL, Redis, and other services

2. **Configure environment variables**
   - Type: Config
   - Priority: Critical
   - Estimate: 1h
   - Description: Create .env.example and document all required variables

3. **Set up database migrations**
   - Type: Config
   - Priority: Critical
   - Estimate: 3h
   - Description: Initialize Prisma, create first migration

4. **Add ESLint and Prettier configuration**
   - Type: Config
   - Priority: High
   - Estimate: 2h
   - Description: Configure linting rules across monorepo

5. **Set up pre-commit hooks (Husky)**
   - Type: Config
   - Priority: High
   - Estimate: 1h
   - Description: Add Git hooks for linting and testing

#### Frontend Setup
6. **Configure Tailwind CSS in web app**
   - Type: Config
   - Priority: High
   - Estimate: 2h
   - Description: Set up Tailwind with custom theme

7. **Integrate shadcn/ui component library**
   - Type: Config
   - Priority: High
   - Estimate: 3h
   - Description: Install and configure shadcn/ui components

8. **Create base layout components**
   - Type: Feature
   - Priority: Medium
   - Estimate: 3h
   - Description: Header, Footer, Sidebar, Container components

9. **Set up React Query (TanStack Query)**
   - Type: Config
   - Priority: High
   - Estimate: 2h
   - Description: Configure query client and providers

10. **Configure Zustand for state management**
    - Type: Config
    - Priority: Medium
    - Estimate: 2h
    - Description: Set up global state store

#### Backend Setup
11. **Configure NestJS validation pipes**
    - Type: Config
    - Priority: High
    - Estimate: 1h
    - Description: Global validation with class-validator

12. **Set up Swagger/OpenAPI documentation**
    - Type: Config
    - Priority: Medium
    - Estimate: 2h
    - Description: Auto-generate API documentation

13. **Configure CORS and security headers**
    - Type: Config
    - Priority: Critical
    - Estimate: 1h
    - Description: Set up proper CORS and helmet

14. **Set up logging (Winston)**
    - Type: Config
    - Priority: Medium
    - Estimate: 2h
    - Description: Structured logging across services

15. **Create health check endpoints**
    - Type: Feature
    - Priority: High
    - Estimate: 1h
    - Description: /health and /readiness endpoints

---

### Phase 1: Authentication & Authorization (15 tasks)

#### Database Schema
16. **Design User schema in Prisma**
    - Type: Feature
    - Priority: Critical
    - Estimate: 2h
    - Description: Users, sessions, preferences tables

17. **Create database seed data**
    - Type: Config
    - Priority: Medium
    - Estimate: 2h
    - Description: Test users and initial data

#### Backend Auth
18. **Implement JWT authentication module**
    - Type: Feature
    - Priority: Critical
    - Estimate: 5h
    - Description: JWT generation, validation, refresh tokens

19. **Create Auth service (register, login, logout)**
    - Type: Feature
    - Priority: Critical
    - Estimate: 5h
    - Description: Core authentication endpoints

20. **Implement password hashing (bcrypt/argon2)**
    - Type: Feature
    - Priority: Critical
    - Estimate: 2h
    - Description: Secure password storage

21. **Add email verification flow**
    - Type: Feature
    - Priority: High
    - Estimate: 5h
    - Description: Verification emails and token validation

22. **Implement password reset flow**
    - Type: Feature
    - Priority: High
    - Estimate: 5h
    - Description: Forgot password and reset functionality

23. **Create Auth guards and decorators**
    - Type: Feature
    - Priority: High
    - Estimate: 3h
    - Description: Route protection and user decorators

24. **Add OAuth2 integration (Google)**
    - Type: Feature
    - Priority: Medium
    - Estimate: 8h
    - Description: Google Sign-In integration

25. **Implement rate limiting**
    - Type: Feature
    - Priority: High
    - Estimate: 2h
    - Description: Protect auth endpoints from brute force

#### Frontend Auth
26. **Create login page**
    - Type: Feature
    - Priority: Critical
    - Estimate: 5h
    - Description: Login form with validation

27. **Create registration page**
    - Type: Feature
    - Priority: Critical
    - Estimate: 5h
    - Description: Sign up form with validation

28. **Implement auth state management**
    - Type: Feature
    - Priority: Critical
    - Estimate: 3h
    - Description: Store user session, auto-refresh tokens

29. **Create protected route wrapper**
    - Type: Feature
    - Priority: Critical
    - Estimate: 2h
    - Description: Higher-order component for auth protection

30. **Build user profile page**
    - Type: Feature
    - Priority: High
    - Estimate: 5h
    - Description: View and edit user profile

---

## Sprint Planning

### Sprint 1 (Week 45)
**Focus**: Complete Phase 0 - Infrastructure

**Tasks** (30 hours):
- Docker Compose setup
- Environment variables
- Database migrations
- Tailwind CSS configuration
- shadcn/ui integration
- React Query setup

**Goal**: Have fully functional development environment

---

### Sprint 2 (Week 46-47)
**Focus**: Begin Phase 1 - Authentication Backend

**Tasks**:
- User schema design
- JWT authentication
- Auth service implementation
- Password flows
- Guards and decorators

**Goal**: Complete backend authentication

---

### Sprint 3 (Week 48)
**Focus**: Phase 1 - Authentication Frontend

**Tasks**:
- Login/register pages
- Auth state management
- Protected routes
- User profile

**Goal**: End-to-end authentication working

---

## Task Linking

### Branch Naming
When you start a task, create a branch and add it to Notion:
```
feature/16-user-schema
feature/26-login-page
fix/auth-token-refresh
```

### PR Linking
When you create a PR, add the link to the task:
```
https://github.com/juanjdev/BrainBudget/pull/1
```

### Commits
Reference tasks in commit messages:
```
feat(auth): implement JWT authentication (#18)
```

---

## Daily Workflow

1. **Morning**: Review "To Do" view, pick highest priority task
2. **Start Task**: Move to "In Progress", create branch, add branch name to Notion
3. **Work**: Develop, test, commit with conventional commits
4. **Finish**: Create PR, link in Notion, move to "Done" when merged
5. **Evening**: Update estimates based on actual time spent

---

## Weekly Review

Every Sunday:
1. Review completed tasks
2. Update journal entry
3. Plan next week's sprint
4. Move tasks from Backlog to To Do
5. Adjust priorities based on learnings

---

**Created**: 2025-10-30
**Last Updated**: 2025-10-30
