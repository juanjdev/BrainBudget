# Phase 0 & 1 - Detailed Task Breakdown

Complete task descriptions with acceptance criteria for Notion import.

---

## Phase 0: Foundation & Setup

### TASK-001: Set up Docker Compose

**Status**: To Do
**Phase**: Phase 0
**Priority**: Critical
**Type**: Config
**Estimate**: 2h
**Tags**: infrastructure, docker, database

#### Description
Create docker-compose.yml file to run PostgreSQL, Redis, and other required services locally for development.

#### Acceptance Criteria
- [ ] docker-compose.yml created with PostgreSQL 15+
- [ ] Redis 7+ service configured
- [ ] Services start successfully with `docker-compose up`
- [ ] Persistent volumes configured for data
- [ ] Health checks configured for all services
- [ ] Environment variables documented
- [ ] Services accessible from host machine

#### Technical Notes
```yaml
# Services to include:
- PostgreSQL (port 5432)
- Redis (port 6379)
- pgAdmin (optional, port 5050)
```

#### Dependencies
- None (first task)

#### Resources
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [PostgreSQL Docker Image](https://hub.docker.com/_/postgres)
- [Redis Docker Image](https://hub.docker.com/_/redis)

---

### TASK-002: Configure environment variables

**Status**: To Do
**Phase**: Phase 0
**Priority**: Critical
**Type**: Config
**Estimate**: 1h
**Tags**: config, security

#### Description
Create .env.example template and document all required environment variables for local and production environments.

#### Acceptance Criteria
- [ ] .env.example created with all variables
- [ ] Comments explaining each variable
- [ ] Separate sections (Database, Redis, JWT, APIs, etc.)
- [ ] .env added to .gitignore
- [ ] Documentation in SETUP.md updated
- [ ] Scripts to validate env variables (optional)

#### Technical Notes
```env
# Required variables:
DATABASE_URL=
REDIS_URL=
JWT_SECRET=
JWT_REFRESH_SECRET=
NEXT_PUBLIC_API_URL=
```

#### Dependencies
- Depends on: TASK-001 (for database URLs)

#### Resources
- [Twelve-Factor App Config](https://12factor.net/config)
- [dotenv Documentation](https://github.com/motdotla/dotenv)

---

### TASK-003: Set up database migrations

**Status**: To Do
**Phase**: Phase 0
**Priority**: Critical
**Type**: Config
**Estimate**: 3h
**Tags**: database, prisma, backend

#### Description
Initialize Prisma ORM in the database library, create initial schema, and set up migration workflow.

#### Acceptance Criteria
- [ ] Prisma installed in database library
- [ ] prisma.schema file created
- [ ] Database connection configured
- [ ] First migration created and applied
- [ ] Prisma Client generated
- [ ] Migration scripts added to package.json
- [ ] Seed script created (basic)

#### Technical Notes
```bash
# Commands to run:
pnpm add -D prisma @prisma/client
npx prisma init
npx prisma migrate dev --name init
npx prisma generate
```

#### Dependencies
- Depends on: TASK-001 (Docker services)
- Depends on: TASK-002 (Environment variables)

#### Resources
- [Prisma Getting Started](https://www.prisma.io/docs/getting-started)
- [Prisma Schema Reference](https://www.prisma.io/docs/reference/api-reference/prisma-schema-reference)

---

### TASK-004: Add ESLint and Prettier configuration

**Status**: To Do
**Phase**: Phase 0
**Priority**: High
**Type**: Config
**Estimate**: 2h
**Tags**: code-quality, linting

#### Description
Configure ESLint and Prettier rules across the entire monorepo for consistent code style.

#### Acceptance Criteria
- [ ] ESLint config extends from Nx recommended
- [ ] Prettier config created (.prettierrc)
- [ ] Import sorting configured
- [ ] TypeScript rules configured
- [ ] React/Next.js specific rules added
- [ ] .eslintignore and .prettierignore configured
- [ ] VSCode settings.json updated
- [ ] All existing code passes linting

#### Technical Notes
```json
// Key rules to configure:
- no-console warnings
- unused vars errors
- import order
- max line length
- trailing comma rules
```

#### Dependencies
- None

#### Resources
- [ESLint Configuration](https://eslint.org/docs/latest/use/configure/)
- [Prettier Options](https://prettier.io/docs/en/options.html)

---

### TASK-005: Set up pre-commit hooks (Husky)

**Status**: To Do
**Phase**: Phase 0
**Priority**: High
**Type**: Config
**Estimate**: 1h
**Tags**: git, code-quality

#### Description
Install and configure Husky with lint-staged to run linting and formatting before commits.

#### Acceptance Criteria
- [ ] Husky installed and initialized
- [ ] lint-staged configured
- [ ] pre-commit hook runs lint on staged files
- [ ] pre-commit hook runs Prettier on staged files
- [ ] pre-commit hook runs type checking
- [ ] Commit blocked if checks fail
- [ ] Documentation updated

#### Technical Notes
```bash
# Install:
pnpm add -D husky lint-staged

# Configure:
npx husky init
```

#### Dependencies
- Depends on: TASK-004 (ESLint/Prettier config)

#### Resources
- [Husky Documentation](https://typicode.github.io/husky/)
- [lint-staged](https://github.com/okonet/lint-staged)

---

### TASK-006: Configure Tailwind CSS in web app

**Status**: To Do
**Phase**: Phase 0
**Priority**: High
**Type**: Config
**Estimate**: 2h
**Tags**: frontend, styling, tailwind

#### Description
Set up Tailwind CSS in the Next.js web app with custom theme configuration.

#### Acceptance Criteria
- [ ] Tailwind CSS installed in web app
- [ ] tailwind.config.js configured
- [ ] Custom colors defined (brand colors)
- [ ] Custom fonts configured
- [ ] Dark mode enabled
- [ ] Global styles in app/globals.css
- [ ] Test component with Tailwind classes
- [ ] IntelliSense working in VSCode

#### Technical Notes
```js
// Custom theme colors:
primary: '#3B82F6'
secondary: '#10B981'
accent: '#F59E0B'
```

#### Dependencies
- None

#### Resources
- [Tailwind CSS Installation](https://tailwindcss.com/docs/installation)
- [Next.js + Tailwind](https://tailwindcss.com/docs/guides/nextjs)

---

### TASK-007: Integrate shadcn/ui component library

**Status**: To Do
**Phase**: Phase 0
**Priority**: High
**Type**: Config
**Estimate**: 3h
**Tags**: frontend, ui, components

#### Description
Install and configure shadcn/ui for pre-built, accessible React components.

#### Acceptance Criteria
- [ ] shadcn/ui initialized in web app
- [ ] components.json configured
- [ ] First 5 components installed (Button, Input, Card, Dialog, Label)
- [ ] Theme customized to match brand
- [ ] Example usage page created
- [ ] Dark mode working with components
- [ ] Documentation added for adding new components

#### Technical Notes
```bash
# Install:
npx shadcn@latest init

# Add components:
npx shadcn@latest add button
npx shadcn@latest add input
npx shadcn@latest add card
```

#### Dependencies
- Depends on: TASK-006 (Tailwind CSS)

#### Resources
- [shadcn/ui Documentation](https://ui.shadcn.com/)
- [shadcn/ui Installation](https://ui.shadcn.com/docs/installation/next)

---

### TASK-008: Create base layout components

**Status**: To Do
**Phase**: Phase 0
**Priority**: Medium
**Type**: Feature
**Estimate**: 3h
**Tags**: frontend, components, ui

#### Description
Build reusable layout components (Header, Footer, Sidebar, Container) for consistent UI structure.

#### Acceptance Criteria
- [ ] Header component with navigation
- [ ] Footer component with links
- [ ] Sidebar component (collapsible)
- [ ] Container component for content width
- [ ] MainLayout component combining all
- [ ] Responsive design (mobile, tablet, desktop)
- [ ] Dark mode support
- [ ] Storybook stories (optional)

#### Technical Notes
```tsx
// Components to create:
- Header (logo, nav, user menu)
- Footer (links, copyright)
- Sidebar (navigation, collapsible)
- Container (max-width wrapper)
- MainLayout (combines all)
```

#### Dependencies
- Depends on: TASK-006 (Tailwind)
- Depends on: TASK-007 (shadcn/ui)

#### Resources
- [Next.js Layouts](https://nextjs.org/docs/app/building-your-application/routing/pages-and-layouts)

---

### TASK-009: Set up React Query (TanStack Query)

**Status**: To Do
**Phase**: Phase 0
**Priority**: High
**Type**: Config
**Estimate**: 2h
**Tags**: frontend, data-fetching, state

#### Description
Configure TanStack Query (React Query) for server state management and data fetching.

#### Acceptance Criteria
- [ ] @tanstack/react-query installed
- [ ] QueryClientProvider configured in root layout
- [ ] DevTools enabled in development
- [ ] Default query options configured
- [ ] Custom hooks for API calls created
- [ ] Error handling configured
- [ ] Example query implemented

#### Technical Notes
```tsx
// Default options:
- staleTime: 5 minutes
- cacheTime: 10 minutes
- retry: 3
- refetchOnWindowFocus: true
```

#### Dependencies
- None

#### Resources
- [TanStack Query Docs](https://tanstack.com/query/latest)
- [React Query with Next.js](https://tanstack.com/query/latest/docs/framework/react/guides/ssr)

---

### TASK-010: Configure Zustand for state management

**Status**: To Do
**Phase**: Phase 0
**Priority**: Medium
**Type**: Config
**Estimate**: 2h
**Tags**: frontend, state-management

#### Description
Set up Zustand for global client-side state management (auth, UI state, etc.).

#### Acceptance Criteria
- [ ] Zustand installed
- [ ] Store structure defined
- [ ] Auth store created (user, token)
- [ ] UI store created (theme, sidebar)
- [ ] Persist middleware configured
- [ ] DevTools integration (optional)
- [ ] TypeScript types defined
- [ ] Example usage in component

#### Technical Notes
```ts
// Stores to create:
- authStore (user, isAuthenticated, login, logout)
- uiStore (theme, sidebarOpen, toggleSidebar)
```

#### Dependencies
- None

#### Resources
- [Zustand Documentation](https://docs.pmnd.rs/zustand/getting-started/introduction)

---

### TASK-011: Configure NestJS validation pipes

**Status**: To Do
**Phase**: Phase 0
**Priority**: High
**Type**: Config
**Estimate**: 1h
**Tags**: backend, validation, nestjs

#### Description
Set up global validation pipes in NestJS for automatic DTO validation.

#### Acceptance Criteria
- [ ] class-validator and class-transformer installed
- [ ] Global ValidationPipe configured in main.ts
- [ ] whitelist and forbidNonWhitelisted enabled
- [ ] transform enabled for automatic type conversion
- [ ] Custom validation decorators created (if needed)
- [ ] Example DTO with validation
- [ ] Test validation with invalid data

#### Technical Notes
```ts
// Configuration:
app.useGlobalPipes(new ValidationPipe({
  whitelist: true,
  forbidNonWhitelisted: true,
  transform: true,
}));
```

#### Dependencies
- None

#### Resources
- [NestJS Validation](https://docs.nestjs.com/techniques/validation)
- [class-validator](https://github.com/typestack/class-validator)

---

### TASK-012: Set up Swagger/OpenAPI documentation

**Status**: To Do
**Phase**: Phase 0
**Priority**: Medium
**Type**: Config
**Estimate**: 2h
**Tags**: backend, documentation, api

#### Description
Configure Swagger/OpenAPI automatic API documentation generation in NestJS.

#### Acceptance Criteria
- [ ] @nestjs/swagger installed
- [ ] SwaggerModule configured in main.ts
- [ ] API documentation accessible at /api/docs
- [ ] DTOs decorated with ApiProperty
- [ ] Controllers decorated with ApiTags
- [ ] Authentication documented
- [ ] Example requests/responses added
- [ ] swagger.json export configured

#### Technical Notes
```ts
// Endpoint: http://localhost:3001/api/docs
// Export: http://localhost:3001/api/docs-json
```

#### Dependencies
- None

#### Resources
- [NestJS OpenAPI](https://docs.nestjs.com/openapi/introduction)

---

### TASK-013: Configure CORS and security headers

**Status**: To Do
**Phase**: Phase 0
**Priority**: Critical
**Type**: Config
**Estimate**: 1h
**Tags**: backend, security, nestjs

#### Description
Set up CORS policy and security headers (Helmet) in NestJS API.

#### Acceptance Criteria
- [ ] CORS configured for frontend URL
- [ ] Helmet middleware installed and configured
- [ ] CSP headers configured
- [ ] HSTS enabled
- [ ] X-Frame-Options set
- [ ] Environment-specific CORS (dev vs prod)
- [ ] Test CORS from frontend

#### Technical Notes
```ts
// Allowed origins:
- http://localhost:3000 (dev)
- https://brainbudget.app (prod)
```

#### Dependencies
- None

#### Resources
- [NestJS CORS](https://docs.nestjs.com/security/cors)
- [Helmet.js](https://helmetjs.github.io/)

---

### TASK-014: Set up logging (Winston)

**Status**: To Do
**Phase**: Phase 0
**Priority**: Medium
**Type**: Config
**Estimate**: 2h
**Tags**: backend, logging, observability

#### Description
Configure structured logging with Winston for better debugging and monitoring.

#### Acceptance Criteria
- [ ] nest-winston installed
- [ ] Winston logger configured
- [ ] Log levels configured (error, warn, info, debug)
- [ ] File transport for production logs
- [ ] Console transport for development
- [ ] Request logging middleware
- [ ] Error logging interceptor
- [ ] Log format standardized (JSON)

#### Technical Notes
```ts
// Log levels:
- error: Errors and exceptions
- warn: Warning messages
- info: General information
- debug: Detailed debugging
```

#### Dependencies
- None

#### Resources
- [nest-winston](https://github.com/gremo/nest-winston)
- [Winston Documentation](https://github.com/winstonjs/winston)

---

### TASK-015: Create health check endpoints

**Status**: To Do
**Phase**: Phase 0
**Priority**: High
**Type**: Feature
**Estimate**: 1h
**Tags**: backend, monitoring, nestjs

#### Description
Implement /health and /readiness endpoints for monitoring and load balancer checks.

#### Acceptance Criteria
- [ ] @nestjs/terminus installed
- [ ] /health endpoint returns 200 OK
- [ ] /readiness endpoint checks database
- [ ] /readiness endpoint checks Redis
- [ ] Response includes service version
- [ ] Response includes uptime
- [ ] Proper error responses for unhealthy state
- [ ] Test with curl/Postman

#### Technical Notes
```json
// Response format:
{
  "status": "ok",
  "info": {
    "database": { "status": "up" },
    "redis": { "status": "up" }
  },
  "version": "0.1.0",
  "uptime": 123456
}
```

#### Dependencies
- Depends on: TASK-003 (Database)
- Depends on: TASK-001 (Redis)

#### Resources
- [NestJS Health Checks](https://docs.nestjs.com/recipes/terminus)

---

## Phase 1: Authentication & Authorization

### TASK-016: Design User schema in Prisma

**Status**: To Do
**Phase**: Phase 1
**Priority**: Critical
**Type**: Feature
**Estimate**: 2h
**Tags**: database, prisma, backend

#### Description
Design and implement the User, Session, and UserPreferences database schema in Prisma.

#### Acceptance Criteria
- [ ] User model defined with all fields
- [ ] Session model for JWT refresh tokens
- [ ] UserPreferences model
- [ ] Proper relations between models
- [ ] Indexes on email and other lookup fields
- [ ] Default values configured
- [ ] Migration created and applied
- [ ] Prisma Client regenerated

#### Technical Notes
```prisma
model User {
  id            String    @id @default(uuid())
  email         String    @unique
  passwordHash  String?
  firstName     String?
  lastName      String?
  avatarUrl     String?
  emailVerified Boolean   @default(false)
  isActive      Boolean   @default(true)
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
  deletedAt     DateTime?

  sessions      Session[]
  preferences   UserPreferences?
}
```

#### Dependencies
- Depends on: TASK-003 (Prisma setup)

#### Resources
- [Prisma Schema](https://www.prisma.io/docs/concepts/components/prisma-schema)
- Project: database-schema.md

---

### TASK-017: Create database seed data

**Status**: To Do
**Phase**: Phase 1
**Priority**: Medium
**Type**: Config
**Estimate**: 2h
**Tags**: database, testing

#### Description
Create seed script to populate database with test users and initial data for development.

#### Acceptance Criteria
- [ ] seed.ts script created
- [ ] Test users created (admin, regular user)
- [ ] Default categories seeded
- [ ] Default payment methods seeded
- [ ] Passwords hashed properly
- [ ] Script can be run multiple times (idempotent)
- [ ] Documentation for seed data
- [ ] npm script added (pnpm db:seed)

#### Technical Notes
```ts
// Test users:
- admin@test.com / password123
- user@test.com / password123
```

#### Dependencies
- Depends on: TASK-016 (User schema)
- Depends on: TASK-020 (Password hashing)

#### Resources
- [Prisma Seeding](https://www.prisma.io/docs/guides/migrate/seed-database)

---

### TASK-018: Implement JWT authentication module

**Status**: To Do
**Phase**: Phase 1
**Priority**: Critical
**Type**: Feature
**Estimate**: 5h
**Tags**: backend, auth, jwt, nestjs

#### Description
Create JWT authentication module with token generation, validation, and refresh token logic.

#### Acceptance Criteria
- [ ] Auth module created in NestJS
- [ ] JWT strategy implemented
- [ ] Access token generation (15 min expiry)
- [ ] Refresh token generation (7 days expiry)
- [ ] Token validation middleware
- [ ] Token refresh endpoint
- [ ] Blacklist for revoked tokens (Redis)
- [ ] Unit tests for token functions

#### Technical Notes
```ts
// Token payload:
{
  sub: userId,
  email: string,
  iat: timestamp,
  exp: timestamp
}
```

#### Dependencies
- Depends on: TASK-016 (User schema)
- Depends on: TASK-002 (JWT secrets)

#### Resources
- [NestJS JWT](https://docs.nestjs.com/security/authentication#jwt-functionality)
- [@nestjs/jwt](https://www.npmjs.com/package/@nestjs/jwt)

---

### TASK-019: Create Auth service (register, login, logout)

**Status**: To Do
**Phase**: Phase 1
**Priority**: Critical
**Type**: Feature
**Estimate**: 5h
**Tags**: backend, auth, nestjs

#### Description
Implement core authentication endpoints: register, login, logout, and refresh token.

#### Acceptance Criteria
- [ ] POST /auth/register endpoint
- [ ] POST /auth/login endpoint
- [ ] POST /auth/logout endpoint
- [ ] POST /auth/refresh endpoint
- [ ] Input validation with DTOs
- [ ] Proper error handling
- [ ] Rate limiting applied
- [ ] Response includes user + tokens
- [ ] Unit and integration tests

#### Technical Notes
```ts
// Endpoints:
POST /auth/register -> { user, accessToken, refreshToken }
POST /auth/login -> { user, accessToken, refreshToken }
POST /auth/logout -> { message }
POST /auth/refresh -> { accessToken, refreshToken }
```

#### Dependencies
- Depends on: TASK-018 (JWT module)
- Depends on: TASK-020 (Password hashing)

#### Resources
- [NestJS Authentication](https://docs.nestjs.com/security/authentication)

---

### TASK-020: Implement password hashing (bcrypt/argon2)

**Status**: To Do
**Phase**: Phase 1
**Priority**: Critical
**Type**: Feature
**Estimate**: 2h
**Tags**: backend, security, auth

#### Description
Implement secure password hashing using Argon2 (or bcrypt) for user passwords.

#### Acceptance Criteria
- [ ] Argon2 or bcrypt library installed
- [ ] Hash function created
- [ ] Compare function created
- [ ] Proper salt rounds configured (10+)
- [ ] Used in register endpoint
- [ ] Used in login verification
- [ ] Performance tested
- [ ] Unit tests for hashing functions

#### Technical Notes
```ts
// Use Argon2 (recommended) or bcrypt
import * as argon2 from 'argon2';

// Hash: argon2.hash(password)
// Verify: argon2.verify(hash, password)
```

#### Dependencies
- None

#### Resources
- [Argon2](https://github.com/ranisalt/node-argon2)
- [OWASP Password Storage](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)

---

### TASK-021: Add email verification flow

**Status**: To Do
**Phase**: Phase 1
**Priority**: High
**Type**: Feature
**Estimate**: 5h
**Tags**: backend, auth, email

#### Description
Implement email verification flow with token generation and validation.

#### Acceptance Criteria
- [ ] Verification token generation
- [ ] Verification email sent on registration
- [ ] GET /auth/verify/:token endpoint
- [ ] Token expiration (24 hours)
- [ ] Mark user as verified
- [ ] Resend verification email endpoint
- [ ] Email template created
- [ ] Integration with email service (Resend/SendGrid)

#### Technical Notes
```ts
// Flow:
1. User registers
2. Generate verification token
3. Send email with link
4. User clicks link
5. Validate token
6. Mark user.emailVerified = true
```

#### Dependencies
- Depends on: TASK-019 (Auth service)
- Blocks: TASK-022 (Password reset)

#### Resources
- [Resend](https://resend.com/docs)
- [Email verification best practices](https://postmarkapp.com/guides/email-verification)

---

### TASK-022: Implement password reset flow

**Status**: To Do
**Phase**: Phase 1
**Priority**: High
**Type**: Feature
**Estimate**: 5h
**Tags**: backend, auth, email

#### Description
Implement forgot password and password reset flow with secure tokens.

#### Acceptance Criteria
- [ ] POST /auth/forgot-password endpoint
- [ ] Reset token generation (1-hour expiry)
- [ ] Reset email sent
- [ ] POST /auth/reset-password endpoint
- [ ] Token validation
- [ ] Password updated
- [ ] Old tokens invalidated
- [ ] Email templates created
- [ ] Rate limiting on forgot-password

#### Technical Notes
```ts
// Flow:
1. User requests reset
2. Generate secure token
3. Send email with link
4. User clicks link
5. User enters new password
6. Validate token
7. Hash and update password
```

#### Dependencies
- Depends on: TASK-021 (Email service)
- Depends on: TASK-020 (Password hashing)

#### Resources
- [Password Reset Best Practices](https://cheatsheetseries.owasp.org/cheatsheets/Forgot_Password_Cheat_Sheet.html)

---

### TASK-023: Create Auth guards and decorators

**Status**: To Do
**Phase**: Phase 1
**Priority**: High
**Type**: Feature
**Estimate**: 3h
**Tags**: backend, auth, nestjs

#### Description
Create reusable auth guards and decorators for protecting routes and accessing user data.

#### Acceptance Criteria
- [ ] JwtAuthGuard created
- [ ] @CurrentUser() decorator created
- [ ] @Public() decorator for public routes
- [ ] @Roles() decorator for RBAC (future)
- [ ] Guards applied globally
- [ ] Test protected endpoints
- [ ] Documentation for usage
- [ ] Example implementations

#### Technical Notes
```ts
// Usage:
@UseGuards(JwtAuthGuard)
@Get('profile')
getProfile(@CurrentUser() user: User) {
  return user;
}

@Public()
@Get('public')
getPublicData() {}
```

#### Dependencies
- Depends on: TASK-018 (JWT module)

#### Resources
- [NestJS Guards](https://docs.nestjs.com/guards)
- [Custom Decorators](https://docs.nestjs.com/custom-decorators)

---

### TASK-024: Add OAuth2 integration (Google)

**Status**: To Do
**Phase**: Phase 1
**Priority**: Medium
**Type**: Feature
**Estimate**: 8h
**Tags**: backend, auth, oauth, google

#### Description
Implement Google OAuth2 Sign-In for easy user authentication.

#### Acceptance Criteria
- [ ] Google OAuth configured in Google Console
- [ ] Passport Google OAuth strategy installed
- [ ] GET /auth/google endpoint (redirect)
- [ ] GET /auth/google/callback endpoint
- [ ] User created/linked on OAuth login
- [ ] JWT tokens issued after OAuth
- [ ] Handle existing email conflicts
- [ ] Frontend button integration
- [ ] Test OAuth flow end-to-end

#### Technical Notes
```ts
// Flow:
1. User clicks "Sign in with Google"
2. Redirect to Google
3. User approves
4. Callback receives user data
5. Create or find user
6. Issue JWT tokens
7. Redirect to frontend with tokens
```

#### Dependencies
- Depends on: TASK-018 (JWT module)
- Depends on: TASK-019 (Auth service)

#### Resources
- [Passport Google OAuth](http://www.passportjs.org/packages/passport-google-oauth20/)
- [Google OAuth Setup](https://developers.google.com/identity/protocols/oauth2)

---

### TASK-025: Implement rate limiting

**Status**: To Do
**Phase**: Phase 1
**Priority**: High
**Type**: Feature
**Estimate**: 2h
**Tags**: backend, security, nestjs

#### Description
Add rate limiting to authentication endpoints to prevent brute force attacks.

#### Acceptance Criteria
- [ ] @nestjs/throttler installed
- [ ] Global rate limiting configured (100 req/15min)
- [ ] Stricter limits on auth endpoints (5 req/15min)
- [ ] Redis storage for distributed rate limiting
- [ ] Custom error messages
- [ ] Rate limit headers in response
- [ ] Test rate limiting triggers
- [ ] Documentation for limits

#### Technical Notes
```ts
// Limits:
- Global: 100 requests per 15 minutes
- /auth/login: 5 requests per 15 minutes
- /auth/register: 5 requests per 15 minutes
- /auth/forgot-password: 3 requests per hour
```

#### Dependencies
- Depends on: TASK-001 (Redis)

#### Resources
- [NestJS Rate Limiting](https://docs.nestjs.com/security/rate-limiting)
- [@nestjs/throttler](https://github.com/nestjs/throttler)

---

### TASK-026: Create login page

**Status**: To Do
**Phase**: Phase 1
**Priority**: Critical
**Type**: Feature
**Estimate**: 5h
**Tags**: frontend, auth, ui

#### Description
Build login page with form validation and error handling.

#### Acceptance Criteria
- [ ] Login page at /login
- [ ] Email and password inputs (shadcn/ui)
- [ ] Form validation (Zod + React Hook Form)
- [ ] "Remember me" checkbox
- [ ] "Forgot password" link
- [ ] Error messages displayed
- [ ] Loading state during login
- [ ] Redirect to dashboard on success
- [ ] "Sign up" link for registration
- [ ] Responsive design
- [ ] Dark mode support

#### Technical Notes
```tsx
// Form fields:
- email (required, email format)
- password (required, min 8 chars)
- rememberMe (optional boolean)

// On submit:
1. Validate
2. Call login API
3. Store tokens
4. Update auth state
5. Redirect to /dashboard
```

#### Dependencies
- Depends on: TASK-019 (Login API)
- Depends on: TASK-007 (shadcn/ui)

#### Resources
- [React Hook Form](https://react-hook-form.com/)
- [Zod](https://zod.dev/)

---

### TASK-027: Create registration page

**Status**: To Do
**Phase**: Phase 1
**Priority**: Critical
**Type**: Feature
**Estimate**: 5h
**Tags**: frontend, auth, ui

#### Description
Build registration page with comprehensive form validation.

#### Acceptance Criteria
- [ ] Registration page at /register
- [ ] Name, email, password, confirm password inputs
- [ ] Form validation (Zod)
- [ ] Password strength indicator
- [ ] Terms of service checkbox
- [ ] Error messages displayed
- [ ] Loading state during registration
- [ ] Success message
- [ ] Redirect to email verification notice
- [ ] "Already have account" link
- [ ] Responsive design

#### Technical Notes
```tsx
// Form fields:
- firstName (required)
- lastName (optional)
- email (required, valid email)
- password (required, min 8 chars, strength check)
- confirmPassword (must match)
- agreeToTerms (required)

// On submit:
1. Validate
2. Call register API
3. Show verification email notice
4. Redirect to check email page
```

#### Dependencies
- Depends on: TASK-019 (Register API)
- Depends on: TASK-007 (shadcn/ui)

#### Resources
- [Password Strength Meter](https://www.npmjs.com/package/zxcvbn)

---

### TASK-028: Implement auth state management

**Status**: To Do
**Phase**: Phase 1
**Priority**: Critical
**Type**: Feature
**Estimate**: 3h
**Tags**: frontend, auth, state

#### Description
Implement global authentication state management with auto token refresh.

#### Acceptance Criteria
- [ ] Auth context/store created (Zustand)
- [ ] User data stored in state
- [ ] Tokens stored securely (httpOnly cookies preferred)
- [ ] Auto token refresh before expiry
- [ ] Login/logout actions
- [ ] Persist auth state across refresh
- [ ] Clear state on logout
- [ ] useAuth hook for components
- [ ] Loading and error states

#### Technical Notes
```ts
// Auth store:
{
  user: User | null,
  isAuthenticated: boolean,
  isLoading: boolean,
  login: (credentials) => Promise<void>,
  logout: () => void,
  refreshToken: () => Promise<void>
}

// Auto refresh 1 min before token expiry
```

#### Dependencies
- Depends on: TASK-010 (Zustand)
- Depends on: TASK-026 (Login page)

#### Resources
- [JWT Refresh Token Pattern](https://hasura.io/blog/best-practices-of-using-jwt-with-graphql/)

---

### TASK-029: Create protected route wrapper

**Status**: To Do
**Phase**: Phase 1
**Priority**: Critical
**Type**: Feature
**Estimate**: 2h
**Tags**: frontend, auth, routing

#### Description
Create higher-order component or middleware for protecting authenticated routes.

#### Acceptance Criteria
- [ ] ProtectedRoute component created
- [ ] Check authentication status
- [ ] Redirect to login if not authenticated
- [ ] Show loading state while checking
- [ ] Preserve intended destination
- [ ] Works with Next.js App Router
- [ ] Type-safe with TypeScript
- [ ] Applied to dashboard routes
- [ ] Test authentication flow

#### Technical Notes
```tsx
// Usage:
<ProtectedRoute>
  <DashboardPage />
</ProtectedRoute>

// Or middleware in Next.js:
// middleware.ts with auth check
```

#### Dependencies
- Depends on: TASK-028 (Auth state)

#### Resources
- [Next.js Middleware](https://nextjs.org/docs/app/building-your-application/routing/middleware)
- [Next.js Authentication](https://nextjs.org/docs/app/building-your-application/authentication)

---

### TASK-030: Build user profile page

**Status**: To Do
**Phase**: Phase 1
**Priority**: High
**Type**: Feature
**Estimate**: 5h
**Tags**: frontend, user, ui

#### Description
Create user profile page where users can view and edit their information.

#### Acceptance Criteria
- [ ] Profile page at /profile
- [ ] Display user information
- [ ] Edit mode with form
- [ ] Avatar upload (optional)
- [ ] Change password section
- [ ] Form validation
- [ ] Save changes API call
- [ ] Success/error messages
- [ ] Loading states
- [ ] Cancel changes button
- [ ] Responsive design

#### Technical Notes
```tsx
// Sections:
- Personal info (name, email)
- Avatar (upload/change)
- Password change
- Account settings
- Preferences (currency, timezone)

// API calls:
- GET /users/me
- PATCH /users/me
- POST /users/me/avatar
```

#### Dependencies
- Depends on: TASK-028 (Auth state)
- Depends on: TASK-029 (Protected routes)

#### Resources
- [File Upload Best Practices](https://web.dev/file-upload-best-practices/)

---

## Summary

**Total Tasks**: 30
- **Phase 0**: 15 tasks (Infrastructure & Setup)
- **Phase 1**: 15 tasks (Authentication & Authorization)

**Total Estimated Time**: ~90 hours (~3-4 weeks full-time)

**Critical Path Tasks** (must be done first):
1. TASK-001 (Docker)
2. TASK-002 (Env vars)
3. TASK-003 (Prisma)
4. TASK-016 (User schema)
5. TASK-018 (JWT)
6. TASK-019 (Auth service)

---

**Created**: 2025-10-30
**Last Updated**: 2025-10-30
