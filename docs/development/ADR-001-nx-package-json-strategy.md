# ADR-001: Nx Monorepo with Individual package.json Files

**Status**: Accepted
**Date**: 2025-11-02
**Decision Makers**: Development Team

## Context

During Phase 0 setup, we integrated shadcn/ui into the web application. However, we encountered a significant limitation: the shadcn/ui CLI does not work with Nx's Single Version Policy (SVP), where all dependencies are managed in a single root `package.json` without individual `package.json` files per project.

### The Problem

The shadcn/ui CLI requires:
1. A `package.json` file in the project directory
2. Framework detection (Next.js, Vite, etc.) based on project structure
3. Ability to install dependencies at the project level

With Nx Single Version Policy:
- No `package.json` exists in individual projects (only at root)
- CLI cannot detect the framework or project structure
- We must manually copy components instead of using `npx shadcn@latest add [component]`

This pattern affects not just shadcn/ui but many modern tooling ecosystems that expect standard project structures.

## Decision

**We will migrate from Nx Single Version Policy to individual `package.json` files per project**, while maintaining the Nx monorepo architecture.

### What This Means

1. **Keep Nx monorepo** - Continue using Nx for:
   - Task orchestration and caching
   - Dependency graph visualization
   - Atomic commits across projects
   - Workspace-wide tooling (ESLint, Prettier, TypeScript)

2. **Add individual package.json** - Each project (`web/`, `api/`, etc.) will have its own:
   - `package.json` with project-specific dependencies
   - Standard framework structure (recognizable by CLIs)
   - npm scripts for common tasks

3. **Hybrid dependency management**:
   - Shared dependencies (TypeScript, ESLint, Prettier) remain at root
   - Project-specific dependencies (Next.js, shadcn/ui, etc.) in project `package.json`
   - Nx handles hoisting and optimization automatically

## Consequences

### Positive

1. **Better tool compatibility**: shadcn/ui CLI and other modern tooling will work as expected
2. **Clearer dependency boundaries**: Each project's dependencies are explicit and visible
3. **Easier onboarding**: Standard structure is more familiar to new developers
4. **Framework updates**: Easier to update framework-specific dependencies
5. **IDE support**: Better autocomplete and type inference for project-specific packages
6. **Ecosystem alignment**: Follows modern monorepo patterns (Turborepo, pnpm workspaces)

### Negative

1. **More configuration files**: Each project has its own `package.json` to maintain
2. **Potential version drift**: Need to be vigilant about keeping shared dependencies aligned
3. **Slightly larger repo**: More files, though negligible in practice
4. **Migration effort**: One-time cost to set up individual `package.json` files

### Neutral

1. **Nx still manages tasks**: No change to how we run builds, tests, etc.
2. **Workspace structure**: Same project organization, just different dependency management
3. **Git workflow**: Same atomic commits and versioning strategy

## Alternatives Considered

### 1. Continue with Single Version Policy (Rejected)

**Pros**:
- Simplest dependency management
- Single source of truth for versions
- Minimal configuration

**Cons**:
- shadcn/ui CLI doesn't work (must manually copy components)
- Many modern tools incompatible
- Fighting against ecosystem trends
- Poor developer experience

### 2. Abandon Nx Monorepo (Rejected)

**Pros**:
- Each app fully independent
- Maximum tool compatibility

**Cons**:
- Lose Nx benefits (caching, task orchestration, dependency graph)
- Code sharing becomes difficult
- More complex deployment and CI/CD
- Studied projects like Ghostfolio prove Nx works well at scale

### 3. Use Different UI Library (Rejected)

**Pros**:
- Could find library compatible with SVP

**Cons**:
- shadcn/ui is industry-leading for customizable components
- Problem extends beyond just UI libraries (affects many tools)
- Doesn't solve root cause

## Implementation Plan

The migration will happen in phases:

### Phase 1: Commit Current Progress (This Session)
- Document this decision (this file)
- Commit Phase 0 progress with shadcn/ui working (manual integration)
- Mark as "learning phase" before architectural migration

### Phase 2: Migrate to Individual package.json (Next Session)
1. Create `web/package.json` with Next.js and web-specific dependencies
2. Create `api/package.json` with NestJS and API-specific dependencies
3. Update root `package.json` to define workspace structure
4. Test that Nx commands still work (`nx dev web`, `nx build api`, etc.)
5. Verify shadcn/ui CLI works after migration
6. Update documentation and README

### Phase 3: Validate and Continue
1. Test full development workflow
2. Ensure CI/CD still works
3. Continue with remaining Phase 0 tasks

## References

### Successful Examples in Production

1. **Ghostfolio** (https://github.com/ghostfolio/ghostfolio)
   - Open-source wealth management app
   - Uses Nx monorepo with Angular + NestJS
   - Individual package.json per project
   - Successfully scales with this architecture

2. **Maybe Finance** (https://github.com/maybe-finance/maybe)
   - Open-source personal finance app
   - Uses Turborepo (similar philosophy)
   - Individual package.json files
   - Proves pattern works for financial apps

### Documentation

- [Nx Package-Based Repos](https://nx.dev/concepts/integrated-vs-package-based)
- [shadcn/ui Installation Guide](https://ui.shadcn.com/docs/installation)
- [Modern Monorepo Best Practices](https://monorepo.tools/)

## Notes

This decision was made after hands-on experience revealed practical limitations with SVP. While SVP is simpler in theory, the ecosystem has moved toward individual package.json files for better tool compatibility. We're aligning with industry best practices while retaining Nx's powerful features.

The fact that successful projects like Ghostfolio use this exact pattern (Nx + individual package.json) gives us confidence this is the right path forward.
