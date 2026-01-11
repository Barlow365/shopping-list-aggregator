# Shopping List Aggregator

| Repo Map | Purpose | Authority |
| --- | --- | --- |
| [docs/PRODUCT_MAP.md](docs/PRODUCT_MAP.md) | Canonical product model (item-first, tag-based) | Source of truth |
| [docs/MASTER_PLAN.md](docs/MASTER_PLAN.md) | End-to-end behavior and execution | Source of truth |
| [docs/EXECUTION_PLAN.md](docs/EXECUTION_PLAN.md) | Technical phasing matrix (Phase 0/1/2) | Source of truth |
| [docs/ux/WIREFRAMES.md](docs/ux/WIREFRAMES.md) | ESW wireframes for all routes | Derived |
| [docs/ux/SITEMAP.md](docs/ux/SITEMAP.md) | Route map tied to PRODUCT_MAP | Derived |
| [docs/planning/MVP_SPEC.md](docs/planning/MVP_SPEC.md) | MVP definition and success criteria | Derived |
| [docs/architecture/ARCHITECTURE.md](docs/architecture/ARCHITECTURE.md) | Tech stack, schema, API contracts | Architecture-only |

| System Entities | Description | Persisted? | User-facing label |
| --- | --- | --- | --- |
| Item | Intended purchase captured as intent | Yes | Thing you need |
| Item Pool (internal) | Global memory storage for items | Yes | Things you need |
| Tag | Store, urgency, budget, person, category | Yes | Tags/filters |
| Store | Shopping location | Yes | Store |
| Budget | Spending limit per period | Yes | Budget |
| Generated View | Contextual list from tags | No (generated) | View |
| Trip | Store/time purchase execution | Yes | Trip |
| Receipt | Purchases recorded for a trip | Yes | Receipt |
| Price Memory | Last paid price per item per store | Yes | Price memory |
| Group (Phase 2) | Shared context | Yes | Group |

| Routes Index | Mode | Writes | Reads | Wireframe location |
| --- | --- | --- | --- | --- |
| /app/intake | ITEM POOL | Items, tags | None | [WIREFRAMES.md](docs/ux/WIREFRAMES.md) (Intake) |
| /app/memory | ITEM POOL | Items, tags | Item Pool, Price Memory | [WIREFRAMES.md](docs/ux/WIREFRAMES.md) (Memory) |
| /app/view | VIEW | View params | Item Pool, Budget | [WIREFRAMES.md](docs/ux/WIREFRAMES.md) (View) |
| /app/trips/start | TRIP | Trip | Item Pool, Stores | [WIREFRAMES.md](docs/ux/WIREFRAMES.md) (Trip Start) |
| /app/trips/:tripId | TRIP | Trip state | Trip | [WIREFRAMES.md](docs/ux/WIREFRAMES.md) (Shop Mode) |
| /app/trips/:tripId/receipts | TRIP | Receipt | Trip, Receipt | [WIREFRAMES.md](docs/ux/WIREFRAMES.md) (Receipts) |

# CANONICAL PRODUCT TRUTH (NO-DRIFT)
The system is ITEM-FIRST.

1) Users add ITEMS first.
2) ITEMS live in a global memory (Things you need).
3) ITEMS are tagged (store, urgency, budget, person, category).
4) LISTS are GENERATED VIEWS of items based on context.
5) LISTS only become concrete when the user is preparing to SHOP.
6) TRIPS are time-bound shopping executions generated from lists.
7) PRICE MEMORY creates a learning loop (receipts update future estimates).

Terminology rules:
- Internal term: "Item Pool" (architecture only).
- User-facing term: "Things you need" or "Your items".
- Store selection is late-bound (happens at View/Trip time, not capture).
- Cart lock-in is prevented by keeping items in memory first.

## Gut Check (Yes/No)
- Can items exist without a list? YES (item-first design)
- Can items exist without a store? YES (late-bound store selection)
- Can users shop at any store? YES (store eligibility filtered at trip time)
- Are items trapped in store-specific lists? NO (cross-pollination mandatory)
- Does price memory track per store? YES (Target $5, Kroger $4.50)
- Can users set budgets? YES (trip, week, or month periods)

## Core Innovation
**Item-First Memory + Late-Bound Context**

Traditional apps: Create list → Add items → Trapped in that list
Our app: Add items → Tag them → Generate views/trips on demand

**Why This Matters**:
- No cart lock-in (items never trapped in store lists)
- Cross-store eligibility (same item can be bought at Target or Kroger)
- Persistent memory (items don't disappear when trip ends)
- Price learning (receipts update future estimates)

## Key Principle
**This is NOT a list-first app, NOT store-specific, NOT cart-based.**

It's an item-first memory system where:
- Items persist across time
- Lists are generated from tags
- Store selection happens late
- Price memory improves estimates
- Users return to "Things you need" (not lists)

---

**Tagline:** Item-first. Tag-based. Late-bound context.

**Documentation:** See [docs/PRODUCT_MAP.md](docs/PRODUCT_MAP.md) for complete feature specifications.

**Technology Stack:** React 18 + TypeScript + Next.js 14 | Node.js 20 + Express | PostgreSQL 16 + Redis 7 | AWS S3

**Status:** Specification Complete → Ready for Phase 0 Development

---

## Development Setup

See [docs/EXECUTION_PLAN.md](docs/EXECUTION_PLAN.md) for phasing details and [docs/architecture/ARCHITECTURE.md](docs/architecture/ARCHITECTURE.md) for tech stack specifications.

## Spot-Check Q&A (Drift Prevention)

| Question | Answer |
| --- | --- |
| Does any doc contradict item-first tagging then generated lists? | NO |
| Does any XO exist without route/state binding? | NO |
| Does MASTER_PLAN contain wireframe dumps? | NO |
| Does every route in SITEMAP have ESW wireframe? | YES |
| Are groups marked as Phase 2 everywhere? | YES |
| Is price memory store-specific? | YES |
| Is budget system defined? | YES |

## Before Coding Checklist

| Requirement | Status |
| --- | --- |
| PRODUCT_MAP defines entities, modes, state machine | ✅ |
| MASTER_PLAN contains blueprint sections | ✅ |
| EXECUTION_PLAN has technical phasing matrix | ✅ |
| ARCHITECTURE has schema, API contracts, tech stack | ✅ |
| WIREFRAMES contain ESW + XO bindings | ✅ |
| SITEMAP routes align to PRODUCT_MAP | ✅ |
| Groups marked as Phase 2 | ✅ |
| Budget system specified | ✅ |
| Price memory is store-specific | ✅ |

**Ready to code**: YES ✅

---

**ESW defines what happens. XO defines how it feels.**

XO must always reference an ESW route + state + forward action. If an experience element is not bound to ESW, it is invalid and must be removed or bound.
