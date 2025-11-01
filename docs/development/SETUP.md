# Development Setup Guide

Complete guide to set up the BrainBudget development environment.

---

## Prerequisites

### Required Software

- **Node.js**: v20.19+ or v22.12+ (for Next.js 16 compatibility)
- **pnpm**: v9+ (package manager)
- **Git**: v2.40+
- **Docker**: v24+ (for local services)
- **Docker Compose**: v2.20+

### Optional Tools

- **VSCode**: Recommended IDE with extensions
- **PostgreSQL**: v15+ (can use Docker instead)
- **Redis**: v7+ (can use Docker instead)

---

## Initial Setup

### 1. Clone Repository

```bash
git clone https://github.com/juanjdev/BrainBudget.git
cd BrainBudget
```

### 2. Install Dependencies

```bash
# Install pnpm globally if not installed
npm install -g pnpm

# Install project dependencies
pnpm install
```

### 3. Environment Variables

```bash
# Copy environment template
cp .env.example .env.local

# Edit with your values
nano .env.local
```

Required variables:
```env
# Database
DATABASE_URL="postgresql://user:password@localhost:5432/brainbudget"

# Redis
REDIS_URL="redis://localhost:6379"

# Next.js
NEXT_PUBLIC_API_URL="http://localhost:3001"

# JWT
JWT_SECRET="your-secret-key-here"
JWT_REFRESH_SECRET="your-refresh-secret-key-here"
```

### 4. Start Services

```bash
# Start PostgreSQL and Redis
docker-compose up -d

# Run database migrations
pnpm db:migrate

# Seed database (optional)
pnpm db:seed
```

### 5. Start Development Servers

```bash
# Start all apps
pnpm dev

# Or individually:
pnpm nx serve web    # Frontend on http://localhost:3000
pnpm nx serve api    # Backend on http://localhost:3001
```

---

## Verification

### Check Installation

```bash
# Verify projects
npx nx show projects

# Should show:
# - web, web-e2e
# - api, api-e2e
# - ui, database, types, validation, utils
```

### Run Tests

```bash
# Run all tests
pnpm test

# Run specific project tests
npx nx test web
npx nx test api
```

### Build Projects

```bash
# Build all projects
pnpm build

# Build specific project
npx nx build web
npx nx build api
```

---

## VSCode Setup

### Recommended Extensions

Install these extensions:

```json
{
  "recommendations": [
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode",
    "bradlc.vscode-tailwindcss",
    "prisma.prisma",
    "nrwl.angular-console",
    "ms-playwright.playwright"
  ]
}
```

### Workspace Settings

Already configured in `.vscode/settings.json`

---

## Troubleshooting

### Port Already in Use

```bash
# Kill process on port 3000
lsof -ti:3000 | xargs kill -9

# Kill process on port 3001
lsof -ti:3001 | xargs kill -9
```

### Database Connection Issues

```bash
# Check Docker containers
docker ps

# Restart services
docker-compose restart

# View logs
docker-compose logs postgres
```

### Node Version Issues

```bash
# Check current version
node --version

# Use nvm to switch versions
nvm install 22.12
nvm use 22.12
```

---

## Next Steps

- Read [BRANCHING-STRATEGY.md](./BRANCHING-STRATEGY.md)
- Check [../QUICK-START.md](../QUICK-START.md) for development guide
- Review [ARCHITECTURE.md](./ARCHITECTURE.md) for system design

---

**Last Updated**: 2025-10-30
