# Recommended Project Structure

```
expense-tracker/
│
├── .github/
│   ├── workflows/
│   │   ├── ci.yml                      # Continuous Integration
│   │   ├── deploy-staging.yml          # Staging deployment
│   │   └── deploy-production.yml       # Production deployment
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug_report.md
│   │   └── feature_request.md
│   └── pull_request_template.md
│
├── apps/
│   ├── web/                            # Next.js Frontend
│   │   ├── app/                        # Next.js 14 App Router
│   │   │   ├── (auth)/
│   │   │   │   ├── login/
│   │   │   │   ├── register/
│   │   │   │   └── layout.tsx
│   │   │   ├── (dashboard)/
│   │   │   │   ├── dashboard/
│   │   │   │   ├── expenses/
│   │   │   │   ├── analytics/
│   │   │   │   ├── budgets/
│   │   │   │   └── layout.tsx
│   │   │   ├── layout.tsx
│   │   │   └── page.tsx
│   │   ├── components/
│   │   │   ├── ui/                     # shadcn/ui components
│   │   │   ├── features/               # Feature components
│   │   │   │   ├── expenses/
│   │   │   │   ├── budgets/
│   │   │   │   └── analytics/
│   │   │   └── layout/
│   │   ├── lib/
│   │   │   ├── api/                    # API client
│   │   │   ├── hooks/                  # Custom hooks
│   │   │   ├── utils/                  # Utility functions
│   │   │   └── validations/            # Form validations
│   │   ├── public/
│   │   │   ├── icons/
│   │   │   └── images/
│   │   ├── styles/
│   │   │   └── globals.css
│   │   ├── .env.local.example
│   │   ├── next.config.js
│   │   ├── tailwind.config.ts
│   │   └── tsconfig.json
│   │
│   ├── api/                            # Backend API (NestJS)
│   │   ├── src/
│   │   │   ├── modules/
│   │   │   │   ├── auth/
│   │   │   │   │   ├── auth.controller.ts
│   │   │   │   │   ├── auth.service.ts
│   │   │   │   │   ├── auth.module.ts
│   │   │   │   │   ├── strategies/
│   │   │   │   │   ├── guards/
│   │   │   │   │   └── dto/
│   │   │   │   ├── users/
│   │   │   │   ├── expenses/
│   │   │   │   ├── categories/
│   │   │   │   ├── budgets/
│   │   │   │   ├── analytics/
│   │   │   │   ├── storage/
│   │   │   │   ├── ocr/
│   │   │   │   ├── ai/
│   │   │   │   └── notifications/
│   │   │   ├── common/
│   │   │   │   ├── decorators/
│   │   │   │   ├── filters/
│   │   │   │   ├── guards/
│   │   │   │   ├── interceptors/
│   │   │   │   ├── pipes/
│   │   │   │   └── middleware/
│   │   │   ├── config/
│   │   │   │   ├── database.config.ts
│   │   │   │   ├── jwt.config.ts
│   │   │   │   └── redis.config.ts
│   │   │   ├── database/
│   │   │   │   ├── migrations/
│   │   │   │   └── seeds/
│   │   │   ├── app.module.ts
│   │   │   └── main.ts
│   │   ├── test/
│   │   │   ├── unit/
│   │   │   ├── integration/
│   │   │   └── e2e/
│   │   ├── .env.example
│   │   ├── nest-cli.json
│   │   └── tsconfig.json
│   │
│   ├── mobile/                         # React Native (Optional)
│   │   ├── src/
│   │   ├── ios/
│   │   ├── android/
│   │   └── package.json
│   │
│   └── worker/                         # Background Jobs Worker
│       ├── src/
│       │   ├── jobs/
│       │   │   ├── ocr-processor.ts
│       │   │   ├── report-generator.ts
│       │   │   ├── email-sender.ts
│       │   │   └── bank-sync.ts
│       │   ├── queues/
│       │   └── index.ts
│       └── package.json
│
├── packages/
│   ├── ui/                             # Shared UI Components
│   │   ├── src/
│   │   │   ├── components/
│   │   │   ├── hooks/
│   │   │   └── index.ts
│   │   ├── package.json
│   │   └── tsconfig.json
│   │
│   ├── database/                       # Prisma Schema
│   │   ├── prisma/
│   │   │   ├── schema.prisma
│   │   │   ├── migrations/
│   │   │   └── seed.ts
│   │   ├── src/
│   │   │   ├── client.ts
│   │   │   └── index.ts
│   │   └── package.json
│   │
│   ├── validation/                     # Zod Schemas
│   │   ├── src/
│   │   │   ├── schemas/
│   │   │   │   ├── auth.schema.ts
│   │   │   │   ├── expense.schema.ts
│   │   │   │   ├── budget.schema.ts
│   │   │   │   └── user.schema.ts
│   │   │   └── index.ts
│   │   └── package.json
│   │
│   ├── types/                          # Shared TypeScript Types
│   │   ├── src/
│   │   │   ├── models/
│   │   │   ├── api/
│   │   │   ├── events/
│   │   │   └── index.ts
│   │   └── package.json
│   │
│   ├── utils/                          # Shared Utilities
│   │   ├── src/
│   │   │   ├── date.ts
│   │   │   ├── currency.ts
│   │   │   ├── formatting.ts
│   │   │   └── index.ts
│   │   └── package.json
│   │
│   └── config/                         # Shared Configuration
│       ├── eslint-config-custom/
│       ├── tsconfig/
│       └── tailwind-config/
│
├── infrastructure/                     # Infrastructure as Code
│   ├── terraform/
│   │   ├── environments/
│   │   │   ├── dev/
│   │   │   ├── staging/
│   │   │   └── production/
│   │   ├── modules/
│   │   │   ├── database/
│   │   │   ├── storage/
│   │   │   ├── networking/
│   │   │   └── compute/
│   │   └── main.tf
│   │
│   ├── kubernetes/                     # K8s Manifests (Optional)
│   │   ├── base/
│   │   └── overlays/
│   │       ├── dev/
│   │       ├── staging/
│   │       └── production/
│   │
│   └── docker/
│       ├── api.Dockerfile
│       ├── web.Dockerfile
│       └── worker.Dockerfile
│
├── docs/                               # Project Documentation
│   ├── architecture/
│   │   ├── architecture-diagrams.md
│   │   ├── microservices-proposal.md
│   │   └── decisions/                  # ADRs
│   │       ├── 001-monorepo.md
│   │       ├── 002-database-choice.md
│   │       └── 003-auth-strategy.md
│   ├── api/
│   │   ├── openapi.yaml
│   │   └── postman-collection.json
│   ├── database/
│   │   ├── database-schema.md
│   │   └── erd.png
│   ├── guides/
│   │   ├── setup.md
│   │   ├── development.md
│   │   ├── deployment.md
│   │   └── contributing.md
│   ├── user-flows.md
│   ├── expense-tracker-plan.md
│   └── learning-roadmap.md
│
├── scripts/                            # Utility Scripts
│   ├── setup.sh
│   ├── deploy.sh
│   ├── seed-database.ts
│   ├── backup.sh
│   └── migrate.sh
│
├── tests/                              # Root-level Tests
│   ├── e2e/
│   ├── integration/
│   └── performance/
│
├── .vscode/                            # VS Code Settings
│   ├── settings.json
│   ├── extensions.json
│   └── launch.json
│
├── .husky/                             # Git Hooks
│   ├── pre-commit
│   ├── pre-push
│   └── commit-msg
│
├── docker-compose.yml                  # Local Development
├── docker-compose.prod.yml             # Production Setup
├── .env.example                        # Environment Variables Template
├── .gitignore
├── .prettierrc
├── .eslintrc.js
├── turbo.json                          # Turborepo Config
├── package.json                        # Root Package
├── pnpm-workspace.yaml                 # pnpm Workspaces
├── README.md                           # Project README
├── CHANGELOG.md                        # Version History
├── CONTRIBUTING.md                     # Contribution Guide
└── LICENSE                             # License File
```

## Key Directory Explanations

### `/apps`
Contains all runnable applications. Each app is independently deployable.

### `/packages`
Shared code that can be imported by apps. Promotes code reuse and consistency.

### `/infrastructure`
All infrastructure code. Keeps deployment configs separate from application code.

### `/docs`
Comprehensive documentation. Single source of truth for project knowledge.

### `/scripts`
Automation scripts for common tasks. Improves developer experience.

## File Naming Conventions

### Components
- PascalCase: `ExpenseCard.tsx`
- Test files: `ExpenseCard.test.tsx`
- Styles: `ExpenseCard.module.css`

### Utilities
- camelCase: `formatCurrency.ts`
- Test files: `formatCurrency.test.ts`

### API Endpoints
- kebab-case: `user-expenses.controller.ts`
- Test files: `user-expenses.controller.spec.ts`

### Database
- snake_case: `expense_categories` (table names)
- camelCase: `expenseCategories` (TypeScript interfaces)

## Git Workflow

### Branch Naming
- Feature: `feature/expense-crud`
- Bug fix: `bugfix/fix-date-validation`
- Hotfix: `hotfix/security-patch`
- Release: `release/v1.0.0`

### Commit Messages
Follow Conventional Commits:
```
feat(expenses): add bulk delete functionality
fix(auth): resolve token refresh race condition
docs(api): update OpenAPI specification
test(budgets): add integration tests
refactor(analytics): optimize query performance
```

## Environment Variables

### Development (.env.local)
```bash
# Database
DATABASE_URL="postgresql://..."
REDIS_URL="redis://..."

# Auth
JWT_SECRET="your-secret-key"
JWT_EXPIRES_IN="7d"
REFRESH_TOKEN_EXPIRES_IN="30d"

# OAuth
GOOGLE_CLIENT_ID="..."
GOOGLE_CLIENT_SECRET="..."

# Storage
AWS_ACCESS_KEY_ID="..."
AWS_SECRET_ACCESS_KEY="..."
AWS_S3_BUCKET="..."

# External Services
OPENAI_API_KEY="..."
PLAID_CLIENT_ID="..."
PLAID_SECRET="..."

# Email
RESEND_API_KEY="..."

# App
NEXT_PUBLIC_API_URL="http://localhost:3001"
NEXT_PUBLIC_APP_URL="http://localhost:3000"
```

## Package Manager

Recommended: **pnpm** for better disk space efficiency and faster installs

```bash
# Install pnpm
npm install -g pnpm

# Install dependencies
pnpm install

# Run dev
pnpm dev

# Build
pnpm build

# Test
pnpm test
```

## Scripts

### Root package.json
```json
{
  "scripts": {
    "dev": "turbo run dev --parallel",
    "build": "turbo run build",
    "test": "turbo run test",
    "lint": "turbo run lint",
    "format": "prettier --write \"**/*.{ts,tsx,md}\"",
    "db:migrate": "cd packages/database && prisma migrate dev",
    "db:seed": "cd packages/database && prisma db seed",
    "db:studio": "cd packages/database && prisma studio",
    "docker:up": "docker-compose up -d",
    "docker:down": "docker-compose down"
  }
}
```

## Development Workflow

1. **Start services**: `pnpm docker:up`
2. **Install dependencies**: `pnpm install`
3. **Run migrations**: `pnpm db:migrate`
4. **Seed database**: `pnpm db:seed`
5. **Start dev servers**: `pnpm dev`
6. **Open app**: http://localhost:3000

## Testing Structure

```
test/
├── unit/                    # Fast, isolated tests
│   ├── services/
│   ├── utils/
│   └── components/
├── integration/             # API integration tests
│   ├── auth/
│   ├── expenses/
│   └── budgets/
├── e2e/                    # End-to-end tests
│   ├── user-flows/
│   └── critical-paths/
└── performance/            # Load tests
    └── k6/
```

## Docker Services

### docker-compose.yml
- PostgreSQL (port 5432)
- Redis (port 6379)
- Mailhog (port 8025) - Email testing
- MinIO (port 9000) - S3-compatible storage

### Service URLs
- Frontend: http://localhost:3000
- Backend API: http://localhost:3001
- API Docs: http://localhost:3001/api-docs
- Database UI: http://localhost:5555 (Prisma Studio)
- Email UI: http://localhost:8025 (Mailhog)
- Storage UI: http://localhost:9001 (MinIO Console)

## CI/CD Pipeline Stages

1. **Lint & Format** - Code quality checks
2. **Type Check** - TypeScript validation
3. **Test** - Run all tests
4. **Build** - Build all apps
5. **Security Scan** - Dependency audit
6. **Deploy to Staging** - Auto-deploy on merge to develop
7. **Deploy to Production** - Manual approval required

## Monitoring & Observability

- **Sentry**: Error tracking
- **PostHog**: Product analytics
- **Prometheus**: Metrics collection
- **Grafana**: Metrics visualization
- **Loki**: Log aggregation

---

This structure is designed for:
✅ Scalability
✅ Maintainability
✅ Team collaboration
✅ Clear separation of concerns
✅ Easy testing
✅ Fast development
✅ Production-ready deployment
