# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**One List** is a budget-aware shared shopping platform with multi-store views, real-time collaboration, and group buying capabilities.

**Tagline:** One list. Every store. Real money control.

**Core Concept:** One shared list with multiple views (by store, aisle, group/tag, recipe, priority, budget) - "multiple lenses on the same truth."

## Primary Documentation

**Start here:** [docs/MASTER_PLAN.md](./docs/MASTER_PLAN.md) - Complete project plan with architecture, features, wireframes, and implementation roadmap.

## Technology Stack

| Component | Technology |
|-----------|------------|
| **Frontend** | React + TypeScript + Next.js 14 (App Router) |
| **UI** | Tailwind CSS + shadcn/ui |
| **State** | Zustand |
| **Forms** | React Hook Form + Zod |
| **Backend** | Node.js + Express |
| **Real-time** | Socket.io |
| **Database** | PostgreSQL + Redis |
| **Auth** | JWT + bcrypt |
| **OCR** | Mindee / AWS Textract |
| **NLP** | OpenAI API / Claude |

## MVP Philosophy

**Full feature set at launch.** No deferred features. Everything ships in MVP:
- Voice capture with NLP parsing
- Receipt OCR with price learning
- Recipe import with ingredient scaling
- Delivery integrations (Instacart, DoorDash)
- Group buying with automatic cost splitting
- Approval rules and spending controls

## Documentation Structure

```
/docs/
├── MASTER_PLAN.md           ← Start here (comprehensive)
├── planning/
│   ├── MVP_SPEC.md
│   ├── ROADMAP.md
│   └── EXECUTION_CHECKLIST.md
├── architecture/
│   ├── ARCHITECTURE.md
│   └── PAGES_COMPONENTS_PLAN.md
├── ux/
│   ├── WIREFRAMES.md
│   └── SITEMAP.md
└── research/
    ├── FEATURES.md
    └── MARKET_ANALYSIS.md
```

## Route Structure

**Marketing:** `/`, `/how-it-works`, `/features`, `/pricing`, `/privacy`, `/terms`

**Auth:** `/login`, `/signup`, `/onboarding`

**App:** `/app/home`, `/app/groups`, `/app/groups/:groupId`, `/app/settings`

**List tabs:** `/app/lists/:listId/items`, `/stores`, `/budget`, `/shop`, `/receipts`, `/history`, `/voice`, `/recipes`, `/recurring`, `/delivery`, `/pooling`, `/controls`

## Core Data Models

- **Users** - Account information
- **Groups** - Households with members
- **Lists** - Shopping lists with budget
- **Items** - List items with priority, store, tags
- **Stores** - Shopping destinations
- **Receipts** - Uploaded receipts with OCR data
- **PriceHistory** - Historical prices from receipts
- **Recipes** - Saved recipes with ingredients
- **RecurringRules** - Auto-add schedules

See [docs/MASTER_PLAN.md](./docs/MASTER_PLAN.md) for full PostgreSQL schema.

## Key Components

| Component | Purpose |
|-----------|---------|
| `budget-spine` | Always-visible budget status panel |
| `quick-add-input` | Fast item entry with comma-split |
| `over-budget-banner` | Smart suggestions when over budget |
| `shop-mode/checklist` | Full-screen in-store shopping mode |
| `receipt-upload` | OCR receipt processing |

## MVP Rules (Non-Negotiables)

- Items come from user input only (no product catalog)
- Store assignment is optional at creation
- One active list per user; archives are read-only
- Budget spine visible on all list surfaces
- Real-time sync < 500ms latency
- Offline mode for core shopping flows

## When Modifying Scope

Update these files on every scope change:
- `docs/MASTER_PLAN.md`
- `docs/ux/SITEMAP.md`
- `docs/architecture/PAGES_COMPONENTS_PLAN.md`
