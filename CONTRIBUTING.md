# CONTRIBUTING

Guidelines for contributing to Shopping List Aggregator.

---

## Before You Start

1. **Read the docs** (15 minutes):
   - [docs/PRODUCT_MAP.md](docs/PRODUCT_MAP.md) - Understand the core model
   - [docs/EXECUTION_PLAN.md](docs/EXECUTION_PLAN.md) - See what phase we're in
   - [CLAUDE.md](CLAUDE.md) - Architecture principles

2. **Set up your environment** (30 minutes):
   - Follow [docs/SETUP.md](docs/SETUP.md)
   - Verify all tests pass: `pnpm run test`

3. **Pick a task**:
   - Check GitHub issues or project board
   - Ask in team chat if unclear

---

## Code Standards

### TypeScript Required

- ✅ All code must be TypeScript (no `.js` files)
- ✅ Strict mode enabled
- ✅ No `any` types (use `unknown` or proper types)
- ✅ Interfaces for all data structures

### Naming Conventions

| Type | Convention | Example |
|------|------------|---------|
| Files | kebab-case | `item-repository.ts` |
| Components | PascalCase | `ItemList.tsx` |
| Functions | camelCase | `createItem()` |
| Constants | UPPER_SNAKE_CASE | `MAX_ITEMS` |

---

## Database Standards

**Always use Drizzle ORM** (no raw SQL)
**Always filter by user_id** (security)
**Always soft delete** (no hard deletes)

```typescript
// ✅ CORRECT
const items = await db.select()
  .from(items)
  .where(and(
    eq(items.user_id, userId),
    eq(items.is_deleted, false)
  ));

// ❌ WRONG - No user_id filter
const items = await db.select().from(items);
```

---

## Git Workflow

### Commit Format

```
type(scope): description

Examples:
feat(items): add quick add with tags
fix(auth): prevent session expiration
docs(setup): add Docker instructions
```

**Types**: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

### Branch Names

- `feature/short-description`
- `fix/issue-number-description`
- `hotfix/description`

---

## Pull Request Checklist

- [ ] Tests pass (`pnpm run test`)
- [ ] No ESLint errors (`pnpm run lint`)
- [ ] Code formatted (`pnpm run format`)
- [ ] User inputs validated with Zod
- [ ] Database queries filter by user_id
- [ ] Documentation updated
- [ ] Screenshots attached (for UI changes)

---

## Testing Standards

**Unit Tests**: Test business logic with Vitest
**Integration Tests**: Test API endpoints
**E2E Tests**: Test critical user flows with Playwright

**Goal**: 80%+ coverage

---

## Security Checklist

- [ ] All API endpoints require authentication
- [ ] All database queries filter by user_id
- [ ] All user inputs validated with Zod
- [ ] No secrets in code
- [ ] No SQL injection vulnerabilities

---

## Questions?

- Check [CLAUDE.md](CLAUDE.md) for architecture guidance
- Check [docs/SETUP.md](docs/SETUP.md) for setup issues
- Ask in team chat
- Open GitHub issue with `question` label

---

**Remember**: Item-first. User isolation. Validate everything. Test everything.

**Last Updated**: 2026-01-11
