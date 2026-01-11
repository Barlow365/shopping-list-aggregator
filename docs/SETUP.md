# DEVELOPMENT ENVIRONMENT SETUP

Complete guide to set up Shopping List Aggregator for local development.

**Goal**: Clone → Setup → Running app in 30 minutes

---

## Prerequisites

### Required Software

| Software | Version | Purpose | Install Command |
|----------|---------|---------|-----------------|
| **Node.js** | 20 LTS | Runtime | [nodejs.org](https://nodejs.org) |
| **pnpm** | Latest | Package manager | `npm install -g pnpm` |
| **PostgreSQL** | 16+ | Database | [postgresql.org](https://www.postgresql.org) |
| **Redis** | 7+ | Caching/sessions | [redis.io](https://redis.io) |
| **Git** | Latest | Version control | [git-scm.com](https://git-scm.com) |

### Optional Software

| Software | Purpose |
|----------|---------|
| **Docker** | Run PostgreSQL/Redis in containers (easier) |
| **Postico** or **pgAdmin** | PostgreSQL GUI |
| **RedisInsight** | Redis GUI |

---

## Quick Start (Using Docker - Recommended)

### 1. Clone Repository

```bash
git clone https://github.com/yourusername/shopping-list-aggregator.git
cd shopping-list-aggregator
```

### 2. Install Dependencies

```bash
pnpm install
```

### 3. Start Services (Docker)

```bash
# Create docker-compose.yml in root:
cat > docker-compose.yml << 'EOF'
version: '3.8'
services:
  postgres:
    image: postgres:16
    environment:
      POSTGRES_DB: shopping_list_dev
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data

volumes:
  postgres_data:
  redis_data:
EOF

# Start services
docker-compose up -d
```

### 4. Configure Environment

```bash
# Copy .env.example to .env
cp .env.example .env

# Edit .env and set:
DATABASE_URL=postgresql://postgres:password@localhost:5432/shopping_list_dev
REDIS_URL=redis://localhost:6379
SESSION_SECRET=$(openssl rand -base64 32)
```

### 5. Run Database Migrations

```bash
# Phase 0: This will be added
# pnpm run db:migrate
```

### 6. Start Development Servers

```bash
# Terminal 1: Start API
cd apps/api
pnpm run dev

# Terminal 2: Start Frontend
cd apps/web
pnpm run dev
```

### 7. Verify Setup

Open browser:
- Frontend: http://localhost:3000
- API: http://localhost:3001/api/health

---

## Manual Setup (Without Docker)

### Install PostgreSQL

#### macOS (Homebrew)
```bash
brew install postgresql@16
brew services start postgresql@16
createdb shopping_list_dev
```

#### Windows
1. Download installer from [postgresql.org](https://www.postgresql.org/download/windows/)
2. Run installer (default port 5432)
3. Remember your password
4. Open pgAdmin and create database `shopping_list_dev`

#### Linux (Ubuntu/Debian)
```bash
sudo apt update
sudo apt install postgresql-16
sudo systemctl start postgresql
sudo -u postgres createdb shopping_list_dev
```

### Install Redis

#### macOS (Homebrew)
```bash
brew install redis
brew services start redis
```

#### Windows
1. Download from [redis.io/download](https://redis.io/download) or use WSL
2. Or use Redis Cloud (free tier): [redis.com/try-free](https://redis.com/try-free)

#### Linux (Ubuntu/Debian)
```bash
sudo apt update
sudo apt install redis-server
sudo systemctl start redis-server
```

---

## Project Structure

```
shopping-list-aggregator/
├── apps/
│   ├── web/           # Next.js frontend (Port 3000)
│   ├── api/           # Express backend (Port 3001)
├── packages/
│   ├── shared/        # Shared types, utilities
│   ├── db/            # Drizzle ORM, migrations
├── docs/              # Documentation (you are here)
├── .env               # Environment variables (DO NOT COMMIT)
├── .env.example       # Template for .env
├── package.json       # Root package.json
├── pnpm-workspace.yaml # pnpm monorepo config
└── turbo.json         # Turborepo config
```

---

## Environment Variables Explained

### Required (Phase 0)

| Variable | Description | Example |
|----------|-------------|---------|
| `DATABASE_URL` | PostgreSQL connection string | `postgresql://postgres:password@localhost:5432/shopping_list_dev` |
| `REDIS_URL` | Redis connection string | `redis://localhost:6379` |
| `SESSION_SECRET` | Random 32-char secret for sessions | Generate: `openssl rand -base64 32` |
| `NODE_ENV` | Environment mode | `development` |
| `API_PORT` | Backend API port | `3001` |
| `NEXT_PUBLIC_API_URL` | Frontend → API URL | `http://localhost:3001/api` |

### Optional (Phase 2)

| Variable | Description | When Needed |
|----------|-------------|-------------|
| `AWS_*` | AWS S3 credentials | Receipt upload (Phase 1) |
| `GOOGLE_SHOPPING_API_KEY` | Google Shopping API | AID feature (Phase 2) |
| `GOOGLE_VISION_API_KEY` | Google Vision API | OCR feature (Phase 2) |
| `SENTRY_DSN` | Error monitoring | Production |
| `NEXT_PUBLIC_POSTHOG_KEY` | Analytics | Production |

---

## Common Setup Issues

### Issue: Port 5432 already in use
**Solution**: Another PostgreSQL instance is running
```bash
# Find process using port 5432
lsof -i :5432
# Kill it or change DATABASE_URL port to 5433
```

### Issue: Cannot connect to PostgreSQL
**Solution**: Check PostgreSQL is running
```bash
# macOS/Linux
brew services list | grep postgresql
# or
systemctl status postgresql

# Windows: Check Services app
```

### Issue: pnpm command not found
**Solution**: Install pnpm globally
```bash
npm install -g pnpm
```

### Issue: Node version mismatch
**Solution**: Use Node 20 LTS
```bash
# Check version
node -v

# Install nvm (Node Version Manager)
# Then: nvm install 20
```

### Issue: Database migration fails
**Solution**: Ensure DATABASE_URL is correct and database exists
```bash
# Test connection
psql postgresql://postgres:password@localhost:5432/shopping_list_dev
```

---

## Development Workflow

### Daily Workflow

1. **Start services** (if not using Docker)
   ```bash
   # PostgreSQL and Redis must be running
   brew services start postgresql@16
   brew services start redis
   ```

2. **Pull latest changes**
   ```bash
   git pull origin main
   pnpm install # Install any new dependencies
   ```

3. **Run migrations** (if any)
   ```bash
   pnpm run db:migrate
   ```

4. **Start dev servers**
   ```bash
   # Option 1: Run all services (Turborepo)
   pnpm run dev

   # Option 2: Run individually
   cd apps/api && pnpm run dev
   cd apps/web && pnpm run dev
   ```

### Available Commands

| Command | Description |
|---------|-------------|
| `pnpm run dev` | Start all services in development mode |
| `pnpm run build` | Build all apps for production |
| `pnpm run test` | Run all tests |
| `pnpm run lint` | Run ESLint on all code |
| `pnpm run format` | Format code with Prettier |
| `pnpm run db:migrate` | Run database migrations |
| `pnpm run db:studio` | Open Drizzle Studio (DB GUI) |

---

## IDE Setup

### VS Code (Recommended)

**Required Extensions**:
- ESLint (`dbaeumer.vscode-eslint`)
- Prettier (`esbenp.prettier-vscode`)
- TypeScript (`ms-vscode.vscode-typescript-next`)

**Recommended Extensions**:
- Tailwind CSS IntelliSense (`bradlc.vscode-tailwindcss`)
- PostgreSQL (`ckolkman.vscode-postgres`)
- GitLens (`eamodio.gitlens`)

**Settings** (`.vscode/settings.json`):
```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "typescript.tsdk": "node_modules/typescript/lib"
}
```

---

## Verification Checklist

Before starting development, verify:

- [ ] Node.js 20 installed (`node -v`)
- [ ] pnpm installed (`pnpm -v`)
- [ ] PostgreSQL running and accessible
- [ ] Redis running and accessible
- [ ] `.env` file created and configured
- [ ] Dependencies installed (`pnpm install`)
- [ ] Database migrations run (Phase 0+)
- [ ] Frontend loads at http://localhost:3000
- [ ] API responds at http://localhost:3001/api/health
- [ ] No console errors in browser or terminal

---

## Next Steps

After setup complete:

1. Read [CONTRIBUTING.md](../CONTRIBUTING.md) for code standards
2. Review [ARCHITECTURE.md](./architecture/ARCHITECTURE.md) for tech stack details
3. Check [EXECUTION_PLAN.md](./EXECUTION_PLAN.md) for current phase tasks
4. Pick a task from Phase 0 and start coding!

---

## Getting Help

**Stuck?** Check:
1. This SETUP.md (common issues above)
2. [ARCHITECTURE.md](./architecture/ARCHITECTURE.md) for technical details
3. Ask in team Slack/Discord
4. Open GitHub issue with `setup` label

---

**Estimated Setup Time**: 30 minutes (with Docker) | 60 minutes (manual)

**Last Updated**: 2026-01-11
