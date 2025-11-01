# ADR-002: Use Next.js 16 for Frontend Application

**Date**: 2025-10-30
**Status**: Accepted
**Deciders**: Juan (Project Owner)
**Tags**: frontend, framework, technology

---

## Context and Problem Statement

What framework should we use for the BrainBudget web application that provides excellent performance, developer experience, SEO capabilities, and modern features while showcasing cutting-edge web development practices?

## Decision Drivers

* Need server-side rendering (SSR) for SEO and performance
* Want to use latest React features (Server Components, Suspense, etc.)
* Require file-based routing for better organization
* Need optimized production builds with minimal configuration
* Want built-in optimizations (images, fonts, scripts)
* Desire for great developer experience with Fast Refresh

## Considered Options

1. **Next.js 16** - Latest version with Turbopack stable
2. **Next.js 14** - Stable LTS version
3. **Remix** - Full-stack React framework
4. **Vite + React Router** - Build tool with routing library
5. **Create React App** - Traditional React setup

## Decision Outcome

**Chosen option**: "Next.js 16"

### Rationale

Next.js 16 provides the most advanced and complete solution:
- **Turbopack stable**: 2-5× faster production builds, 10× faster Fast Refresh
- **React 19 support**: Access to latest React features (Actions, use hook, etc.)
- **New caching model**: `"use cache"` directive for granular control
- **App Router**: Modern routing with layouts, loading states, and error boundaries
- **Server Components**: Optimal performance with zero-bundle JavaScript for static parts
- **Cutting-edge**: Demonstrates knowledge of latest web technologies

### Positive Consequences

* **Performance**: Turbopack provides significantly faster builds and HMR
* **Modern Features**: Can use React 19 Server Actions, async components, etc.
* **Developer Experience**: Fast Refresh is near-instant with Turbopack
* **SEO**: Built-in SSR and SSG for optimal search engine performance
* **Type Safety**: Excellent TypeScript support out of the box
* **Ecosystem**: Large community and extensive plugin ecosystem
* **Learning**: Experience with latest web development practices

### Negative Consequences

* **Bleeding Edge**: Potential bugs or breaking changes in early releases
* **Documentation**: Some features may have limited examples initially
* **Dependencies**: React 19 is new, some libraries may need updates
* **Migration Complexity**: Moving to newer versions later might have breaking changes
* **Node Version**: Requires Node.js 20.19+ or 22.12+

## Pros and Cons of the Options

### Next.js 16

* **Good**, because Turbopack is stable and much faster
* **Good**, because React 19 support with latest features
* **Good**, because new caching primitives for better control
* **Good**, because demonstrates cutting-edge knowledge
* **Bad**, because very new (October 2025 release)
* **Bad**, because potential early-version bugs

### Next.js 14

* **Good**, because very stable and well-tested
* **Good**, because extensive documentation and examples
* **Good**, because all libraries are compatible
* **Bad**, because Turbopack still in beta
* **Bad**, because missing latest React 19 features
* **Bad**, because slower builds compared to v16

### Remix

* **Good**, because excellent data loading patterns
* **Good**, because great for web standards
* **Bad**, because smaller ecosystem than Next.js
* **Bad**, because less integrated tooling
* **Bad**, because steeper learning curve

### Vite + React Router

* **Good**, because very fast development experience
* **Good**, because simple and flexible
* **Bad**, because manual SSR configuration required
* **Bad**, because no built-in optimizations
* **Bad**, because requires more setup and configuration

### Create React App

* **Good**, because simple to start
* **Good**, because well-documented
* **Bad**, because no SSR/SSG support
* **Bad**, because slower builds
* **Bad**, because deprecated by React team

## Links

* Related to [ADR-001: Nx Monorepo](./001-nx-monorepo.md)
* Related to [ADR-003: Tailwind CSS](./003-tailwind-css.md)

---

## Notes

- Using App Router (not Pages Router)
- Configured with TypeScript for type safety
- Integrated with Nx for monorepo builds
- Will need to handle `async params` and `searchParams` (breaking change from v14/15)
