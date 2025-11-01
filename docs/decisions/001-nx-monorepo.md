# ADR-001: Use Nx Monorepo for Project Structure

**Date**: 2025-10-30
**Status**: Accepted
**Deciders**: Juan (Project Owner)
**Tags**: architecture, tooling, monorepo

---

## Context and Problem Statement

How should we structure the BrainBudget codebase to support multiple applications (web frontend, API backend, mobile app) and shared libraries while maintaining code reusability, type safety, and developer productivity?

## Decision Drivers

* Need to share code between frontend and backend (types, validation, utilities)
* Plan to build multiple applications (web, mobile, potentially admin panel)
* Want consistent tooling and build processes across all projects
* Need efficient CI/CD with smart caching and affected builds
* Desire for strong TypeScript integration across all packages

## Considered Options

1. **Nx Monorepo** - Integrated monorepo tool with build system orchestration
2. **Turborepo** - Fast monorepo build system
3. **Lerna** - Traditional JavaScript monorepo tool
4. **pnpm Workspaces** - Native package manager workspace support
5. **Separate Repositories** - Polyrepo approach

## Decision Outcome

**Chosen option**: "Nx Monorepo"

### Rationale

Nx provides the most comprehensive solution for our needs:
- Built-in generators for creating apps and libraries with best practices
- Intelligent dependency graph and affected command for optimal CI/CD
- Integrated caching (local and remote) for faster builds
- Excellent TypeScript support out of the box
- Strong ecosystem with plugins for Next.js, NestJS, React, etc.
- Active development and community support
- Built-in task orchestration and parallel execution

### Positive Consequences

* **Code Sharing**: Easy to share types, utilities, and components across apps
* **Type Safety**: Full TypeScript support from frontend to backend
* **Developer Experience**: Consistent commands (nx serve, nx build, nx test)
* **Performance**: Cached builds and smart affected detection speeds up CI/CD
* **Scalability**: Can easily add new apps/libraries as project grows
* **Documentation**: Integrated graph visualization (nx graph)

### Negative Consequences

* **Learning Curve**: Team needs to learn Nx-specific concepts
* **Complexity**: More complex than simple multi-package setup
* **Lock-in**: Switching away from Nx later would require significant refactoring
* **Bundle Size**: Nx adds dependencies to the project

## Pros and Cons of the Options

### Nx Monorepo

* **Good**, because comprehensive tooling and generators
* **Good**, because intelligent build caching and affected commands
* **Good**, because strong TypeScript integration
* **Good**, because active community and ecosystem
* **Bad**, because steeper learning curve
* **Bad**, because adds framework-specific patterns

### Turborepo

* **Good**, because very fast build performance
* **Good**, because simpler mental model than Nx
* **Bad**, because fewer integrated generators and tooling
* **Bad**, because less mature ecosystem for our stack

### Lerna

* **Good**, because well-established tool
* **Bad**, because less active development
* **Bad**, because no built-in build system orchestration
* **Bad**, because requires additional configuration for optimal performance

### pnpm Workspaces

* **Good**, because native to package manager
* **Good**, because minimal overhead
* **Bad**, because no build orchestration
* **Bad**, because no generators or tooling
* **Bad**, because manual setup for each project

### Separate Repositories

* **Good**, because complete independence
* **Good**, because simpler individual projects
* **Bad**, because code duplication
* **Bad**, because version sync challenges
* **Bad**, because difficult to refactor across projects

## Links

* Related to [ADR-002: Next.js 16](./002-nextjs-16.md)
* Related to [ADR-003: Tailwind CSS](./003-tailwind-css.md)

---

## Notes

- Using pnpm as the package manager within Nx workspace
- All projects use TypeScript for type safety
- Configured with ESLint and Prettier for code quality
