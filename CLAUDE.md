# CLAUDE CODE GUIDANCE

Instructions for Claude Code when working on Shopping List Aggregator.

---

## Core Principle: ITEM-FIRST Architecture

**This is NOT a list-first app. This is NOT store-specific. This is NOT cart-based.**

**Core Model**: ITEM POOL → TAGS → GENERATED VIEWS → TRIPS

### Critical Rules

1. **Items are captured FIRST** (before any list exists)
2. **Store selection is LATE-BOUND** (happens at View/Trip time, not capture)
3. **Lists are GENERATED from tags** (not created upfront)
4. **Items persist in global memory** (never trapped in store-specific lists)
5. **Cross-store eligibility is mandatory** (items can be bought at any eligible store)

### User-Facing Terminology

| Internal Term | User-Facing Label | Use When |
|---------------|-------------------|----------|
| Item Pool | "Things you need" | Always (never say "Item Pool" to users) |
| Generated View | "View" or "List" | Filtering/viewing items |
| Late-bound | (Don't mention) | Store selection happens at trip time |

---

## Documentation Hierarchy (NO-DRIFT Standards)

### Source of Truth (Read FIRST)

1. **[docs/PRODUCT_MAP.md](docs/PRODUCT_MAP.md)** - Canonical product model
   - Defines ALL entities, modes, state machine
   - Single place where features are defined
   - Read this BEFORE writing any code

2. **[docs/MASTER_PLAN.md](docs/MASTER_PLAN.md)** - End-to-end behavior
   - User journey through the system
   - Phase-based implementation approach

3. **[docs/EXECUTION_PLAN.md](docs/EXECUTION_PLAN.md)** - Technical phasing
   - Phase 0/1/2 implementation matrix
   - Dependencies and exit criteria

### Derived Documentation (Reference)

- **[docs/ux/WIREFRAMES.md](docs/ux/WIREFRAMES.md)** - ESW wireframes for all routes
- **[docs/ux/SITEMAP.md](docs/ux/SITEMAP.md)** - Route map tied to PRODUCT_MAP
- **[docs/architecture/ARCHITECTURE.md](docs/architecture/ARCHITECTURE.md)** - Schema, API contracts, tech stack
- **[docs/planning/MVP_SPEC.md](docs/planning/MVP_SPEC.md)** - MVP rules and constraints
- **[docs/planning/ROADMAP.md](docs/planning/ROADMAP.md)** - Timeline and budget

### Setup Documentation

- **[docs/SETUP.md](docs/SETUP.md)** - Development environment setup
- **[CONTRIBUTING.md](CONTRIBUTING.md)** - Code standards and workflow

---

## Tech Stack

### Frontend
- React 18 + TypeScript + Next.js 14 (App Router)
- Zustand (state management)
- TanStack Query (server state)
- Tailwind CSS + shadcn/ui
- Workbox (offline/PWA)

### Backend
- Node.js 20 + Express + TypeScript
- Drizzle ORM (PostgreSQL)
- Lucia Auth (session-based)
- Zod (validation)

### Database
- PostgreSQL 16 (primary)
- Redis 7 (sessions, cache)
- AWS S3 (receipt images)

### Infrastructure
- Vercel (frontend) + Railway (backend + DB)
- GitHub Actions (CI/CD)
- Sentry (errors) + PostHog (analytics)

---

## Code Generation Guidelines

### When Writing Code

1. **Read docs/PRODUCT_MAP.md FIRST** to understand entities and relationships
2. **Check docs/EXECUTION_PLAN.md** to see what phase you're implementing
3. **Follow docs/architecture/ARCHITECTURE.md** for schema and API contracts
4. **Reference docs/ux/WIREFRAMES.md** for UI structure

### Database Queries

- **ALWAYS use Drizzle ORM** (never raw SQL unless absolutely necessary)
- **ALWAYS soft-delete items** (`is_deleted = TRUE`, not DELETE)
- **ALWAYS filter by user_id** (data isolation)
- **ALWAYS include indexes** on foreign keys and commonly filtered fields

Example:
```typescript
// ✅ CORRECT - Soft delete with user check
await db.update(items)
  .set({ is_deleted: true, deleted_at: new Date() })
  .where(and(eq(items.id, itemId), eq(items.user_id, userId)));

// ❌ WRONG - Hard delete
await db.delete(items).where(eq(items.id, itemId));
```

### API Endpoints

- **ALWAYS validate with Zod** (shared schemas between frontend/backend)
- **ALWAYS check authentication** (use Lucia middleware)
- **ALWAYS rate-limit** (100 requests per 15 minutes)
- **ALWAYS return consistent error format**

Example:
```typescript
// ✅ CORRECT - Full validation and auth
router.post('/api/items', requireAuth, async (req, res) => {
  const result = createItemSchema.safeParse(req.body);
  if (!result.success) {
    return res.status(400).json({ error: result.error });
  }
  const item = await createItem(req.userId, result.data);
  return res.json({ item });
});
```

### Frontend State Management

- **Use Zustand for client state** (UI state, offline queue)
- **Use TanStack Query for server state** (API data, caching)
- **ALWAYS handle offline mode** (queue mutations, show offline indicator)

Example:
```typescript
// ✅ CORRECT - TanStack Query with offline support
const { data: items, isLoading } = useQuery({
  queryKey: ['items', userId],
  queryFn: () => fetchItems(userId),
  staleTime: 5 * 60 * 1000, // 5 minutes
});
```

---

## Phase-Specific Guidelines

### Phase 0 (Scaffolding)
- Create routes with stubs (empty states)
- Set up database schema (all tables)
- Implement auth system (Lucia)
- Configure feature flags
- **Goal**: Deployable skeleton (nothing functional yet)

### Phase 1 (MVP)
- Implement manual workflows (no AI, no automation)
- Focus on proving the loop works (capture → trip → receipt → price memory)
- Desktop-first (mobile can wait)
- No external APIs (keep costs zero)
- **Goal**: 10 users complete full loop

### Phase 2 (Automation)
- Add AID (Google Shopping API)
- Add OCR (Google Vision API)
- Add voice input (Web Speech API)
- Add auto-categorization
- **Goal**: Polished product ready for launch

---

## Wireframe Standards (ESW - Executable Structural Wireframes)

### Required Elements
- **HEADER line** with route name
- **MODE indicator** (ITEM POOL, VIEW, TRIP)
- **2-3 columns** showing data flow
- **Symbols**: `[ ]` pending, `[x]` confirmed, `[>]` active, `[?]` suggested

### Example
```
+==================================================================+
| /app/intake | ITEM POOL MODE                                     |
+==================================================================+
| INPUT                 ||| TAGGING              ||| ITEM POOL     |
| [Quick Add Field]     ||| [Store: Target]      ||| Desk Lamp     |
| "Desk Lamp"           ||| [Urgency: High]      ||| Milk          |
|                       ||| [Budget: Office]     ||| Dog Food      |
+==================================================================+
```

---

## Common Mistakes to Avoid

### ❌ List-First Thinking
```typescript
// ❌ WRONG - Creating lists first
await createList({ name: "Target List", store_id: targetId });

// ✅ CORRECT - Items first, views generated
await createItem({ name: "Milk", tags: [{ type: 'store', value: 'Target' }] });
```

### ❌ Store Lock-In
```typescript
// ❌ WRONG - Item trapped in Target list
await createItem({ name: "Milk", list_id: targetListId });

// ✅ CORRECT - Item eligible at any store with milk
await createItem({ name: "Milk", tags: [{ type: 'category', value: 'dairy' }] });
```

### ❌ Hard Delete
```typescript
// ❌ WRONG - Permanent deletion
await db.delete(items).where(eq(items.id, itemId));

// ✅ CORRECT - Soft delete
await db.update(items)
  .set({ is_deleted: true, deleted_at: new Date() })
  .where(eq(items.id, itemId));
```

### ❌ No User Isolation
```typescript
// ❌ WRONG - No user check (security issue!)
const items = await db.select().from(items);

// ✅ CORRECT - Always filter by user_id
const items = await db.select().from(items)
  .where(and(eq(items.user_id, userId), eq(items.is_deleted, false)));
```

---

## Testing Guidelines

### Unit Tests (Vitest)
- Test all business logic functions
- Mock database calls
- Aim for 80%+ coverage

### Integration Tests
- Test API endpoints with test database
- Test authentication flows
- Test offline mode

### E2E Tests (Playwright)
- Test critical user flows:
  1. Signup → Add items → Create trip → Complete trip
  2. Upload receipt → Match items → Price memory updates
- Run on Chrome, Firefox, Safari

---

## Security Checklist

- [ ] All API endpoints require authentication
- [ ] All database queries filter by user_id
- [ ] All user inputs validated with Zod
- [ ] SQL injection prevented (using ORM)
- [ ] XSS prevented (React escapes by default)
- [ ] CSRF tokens on state-changing operations
- [ ] Rate limiting enabled (100 req/15min)
- [ ] File uploads limited to 10MB, images only
- [ ] Passwords hashed with Argon2id
- [ ] Sessions stored in Redis with expiration

---

## Performance Guidelines

- [ ] Database queries use indexes
- [ ] API responses paginated (max 100 items)
- [ ] Images optimized before S3 upload
- [ ] Static assets cached (Vercel CDN)
- [ ] Redis caching for expensive queries
- [ ] Frontend code-split by route
- [ ] Bundle size < 200KB (gzipped)

---

## Deployment Checklist

### Before Deploy
- [ ] All tests passing
- [ ] No console errors or warnings
- [ ] Environment variables set in Vercel/Railway
- [ ] Database migrations run on production DB
- [ ] Sentry configured for error tracking
- [ ] PostHog configured for analytics

### After Deploy
- [ ] Smoke test all critical flows
- [ ] Check Sentry for errors
- [ ] Monitor PostHog for user behavior
- [ ] Check database query performance
- [ ] Verify offline mode works

---

## Git Workflow

### Commit Message Format
```
type(scope): description

[optional body]

[optional footer]
```

**Types**: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

**Examples**:
- `feat(items): add quick add with tags`
- `fix(auth): prevent session expiration on active use`
- `docs(setup): add Docker instructions`

### Branch Strategy
- `main` - Production (always deployable)
- `develop` - Integration branch
- `feature/*` - Feature branches
- `fix/*` - Bug fix branches

### Pull Request Checklist
- [ ] Tests added/updated
- [ ] Documentation updated
- [ ] No console errors
- [ ] Code follows ESLint rules
- [ ] PR description explains changes
- [ ] Screenshots for UI changes

---

## Quick Reference

### Key Files
- **Schema**: `packages/db/schema.ts`
- **API Routes**: `apps/api/src/routes/`
- **Frontend Pages**: `apps/web/app/`
- **Shared Types**: `packages/shared/types.ts`
- **Validation**: `packages/shared/validation.ts`

### Common Commands
```bash
pnpm run dev          # Start all services
pnpm run test         # Run tests
pnpm run lint         # Lint code
pnpm run format       # Format code
pnpm run db:migrate   # Run migrations
pnpm run db:studio    # Open Drizzle Studio
```

### Useful Links
- [Drizzle ORM Docs](https://orm.drizzle.team)
- [Next.js 14 Docs](https://nextjs.org/docs)
- [Lucia Auth Docs](https://lucia-auth.com)
- [TanStack Query Docs](https://tanstack.com/query)
- [Tailwind CSS Docs](https://tailwindcss.com)

---

## When in Doubt

1. **Read docs/PRODUCT_MAP.md** - Is this feature defined there?
2. **Check docs/EXECUTION_PLAN.md** - What phase are you implementing?
3. **Review docs/architecture/ARCHITECTURE.md** - What's the schema/API contract?
4. **Ask the team** - Don't make architectural assumptions

---

**Remember**: Item-first. Tag-based. Late-bound context. NO cart lock-in. NO store-specific lists.

**Last Updated**: 2026-01-11
