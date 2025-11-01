# ADR-003: Use Tailwind CSS for Styling

**Date**: 2025-10-30
**Status**: Accepted
**Deciders**: Juan (Project Owner)
**Tags**: frontend, styling, ui

---

## Context and Problem Statement

What styling solution should we use for BrainBudget that provides rapid development, consistency, responsive design, and modern UI patterns while maintaining good performance and developer experience?

## Decision Drivers

* Need rapid UI development with consistent design system
* Want responsive design that works across all devices
* Require dark mode support
* Need tree-shaking to minimize CSS bundle size
* Want to use component library (shadcn/ui) for pre-built components
* Desire for good TypeScript/autocomplete support in editors

## Considered Options

1. **Tailwind CSS** - Utility-first CSS framework
2. **CSS Modules** - Scoped CSS with PostCSS
3. **Styled Components** - CSS-in-JS library
4. **Emotion** - CSS-in-JS with better performance
5. **Vanilla CSS/SCSS** - Traditional stylesheets

## Decision Outcome

**Chosen option**: "Tailwind CSS"

### Rationale

Tailwind CSS provides the best combination of productivity and performance:
- **Utility-First**: Rapid development without leaving HTML/JSX
- **Consistency**: Built-in design system with spacing, colors, typography
- **Tree-Shaking**: Only CSS for classes actually used ships to production
- **Responsive**: Mobile-first breakpoints out of the box
- **Dark Mode**: Native dark mode support with `dark:` prefix
- **shadcn/ui Compatible**: Perfect match for component library
- **Ecosystem**: Huge community, plugins, and resources

### Positive Consequences

* **Fast Development**: Build UIs quickly with utility classes
* **Consistency**: Design tokens ensure visual consistency
* **Performance**: Optimized CSS bundles (typically <10KB gzipped)
* **Maintainability**: No context switching between HTML and CSS files
* **Responsive**: Easy to create responsive layouts with breakpoint utilities
* **Dark Mode**: Simple dark mode implementation
* **Component Library**: Can use shadcn/ui for pre-built, accessible components
* **Developer Experience**: IntelliSense support in VSCode

### Negative Consequences

* **HTML Verbosity**: Class names can get long
* **Learning Curve**: Need to learn Tailwind's class naming conventions
* **Customization**: Deep customization requires Tailwind config knowledge
* **Debugging**: Can be harder to debug compared to traditional CSS

## Pros and Cons of the Options

### Tailwind CSS

* **Good**, because rapid development with utilities
* **Good**, because built-in design system
* **Good**, because excellent tree-shaking
* **Good**, because great dark mode support
* **Good**, because works perfectly with shadcn/ui
* **Bad**, because verbose class names
* **Bad**, because learning curve for conventions

### CSS Modules

* **Good**, because scoped styles prevent conflicts
* **Good**, because familiar CSS syntax
* **Bad**, because manual responsive design
* **Bad**, because no built-in design system
* **Bad**, because larger final bundle

### Styled Components

* **Good**, because component-scoped styles
* **Good**, because can use JavaScript for dynamic styles
* **Bad**, because runtime overhead
* **Bad**, because larger bundle size
* **Bad**, because no server components support

### Emotion

* **Good**, because better performance than styled-components
* **Good**, because flexible API
* **Bad**, because still has runtime cost
* **Bad**, because requires additional configuration
* **Bad**, because less popular than alternatives

### Vanilla CSS/SCSS

* **Good**, because full control and familiar
* **Good**, because no framework overhead
* **Bad**, because manual design system
* **Bad**, because class name conflicts
* **Bad**, because slower development
* **Bad**, because harder to maintain consistency

## Links

* Related to [ADR-001: Nx Monorepo](./001-nx-monorepo.md)
* Related to [ADR-002: Next.js 16](./002-nextjs-16.md)
* Will be used with shadcn/ui component library

---

## Notes

- Configured in the `ui` library for shared components
- Using Tailwind v3+ with JIT (Just-In-Time) compiler
- Custom configuration in `tailwind.config.js` for brand colors
- Plan to integrate shadcn/ui for pre-built accessible components
- Using PostCSS for additional processing
