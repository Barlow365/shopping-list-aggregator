# MASTER PLAN

# Executive Summary
We are building an item-first memory and purchase assembly system. Users express intent as items, items persist, and lists are generated only when preparing to purchase. The measurable win: users return to "Things you need", generate trips, and close receipts to update price memory.
Source of truth: `docs/PRODUCT_MAP.md`

## Table of Contents
1) Market / Competitive Landscape
2) User Requirements
3) Core Innovation
4) System Model
5) Architecture Design
6) Feature System
7) MVP Rules
8) Feature Milestones
9) Database Schema
10) Critical Files / Repo Structure
11) Complete Application Sitemap
12) Open Questions / Decisions
13) Sources

## Market / Competitive Landscape
| Category | Typical behavior | Our positioning |
| --- | --- | --- |
| Grocery lists | List-first, store-specific | Item-first memory + generated trip |
| Shared lists | Collaboration without price memory | Group efficiency + price memory loop |
| Receipt apps | After-the-fact, single-store | Receipt loop informs future trips |

Positioning Graphic (ASCII)

            High Automation
                   ^
                   |
   Receipt Apps    |        Our System
                   |
   Shared Lists    |  (Item-first + Trip)
                   +----------------------> Decision Quality
                     Low                 High

## User Requirements
| Need | Why it matters |
| --- | --- |
| Remember intent across time | Prevents re-entry and list churn |
| Decide by budget and priority | Forces tradeoffs early |
| Assemble trips by context | Reduces store lock-in |
| Close loop on actuals | Improves future estimates |

## Core Innovation
Item-first capture + tagging creates a persistent memory. Lists are generated only in context (trip/store/time). Group efficiency emerges from pooling items before committing to a store.

## System Model
| Object | Description | Persisted |
| --- | --- | --- |
| Item | Intended purchase captured as intent | Yes |
| Tag | Store, urgency, budget, person, category | Yes |
| Store | Shopping context, not a container | Yes |
| Trip | Store/time execution | Yes |
| Receipt | Purchase record | Yes |
| Group | Shared context (future) | Yes |
| Budget | Constraint applied to views/trips | Yes |

## Architecture Design
| Layer | Notes |
| --- | --- |
| Client | Web-first UI with ESW + XO separation |
| API | CRUD for items, tags, trips, receipts |
| Data | Item memory, tags, receipts, price memory |

Architecture Diagram (ASCII)

[Client UI]
   |  ESW + XO
   v
[API Layer] -> [Item Memory / Tags]
        \-> [Trips / Receipts / Price Memory]

## Feature System
| Stage | Features |
| --- | --- |
| Capture | Intake, AID, item tagging |
| Tag | Store, urgency, budget, person |
| View | Budget/priority/context views |
| Generate Trip | Store selection + trip assembly |
| Shop | Offline checklist + swipe confirm |
| Close Loop | Receipt matching + price memory |

## MVP Rules (Non-Negotiables)
- Items are captured before any list exists.
- Store selection is late-bound at View/Trip time.
- Trips are generated from memory, not vice versa.
- Receipts update price memory and inform future estimates.

## Feature Milestones (Phased Build)
| Phase | Scope | Exit Criteria |
| --- | --- | --- |
| Phase 0 | ESW + XO aligned | Routes mapped, no drift |
| Phase 1 | Capture + Tag + Memory | Items persist + tags saved |
| Phase 2 | Views + Budget | Views generated, budget state exists |
| Phase 3 | Trip + Shop | Trip generation + shop flow used |
| Phase 4 | Receipts + Price Memory | Matching updates prices |

## Database Schema (High-level)
| Table | Fields |
| --- | --- |
| items | id, name, notes, created_by, created_at |
| tags | id, item_id, type, value |
| trips | id, store, created_at, status |
| trip_items | trip_id, item_id, status |
| receipts | id, trip_id, total, created_at |
| price_memory | item_id, last_price, last_seen |
| groups (future) | id, name |

## Critical Files / Repo Structure
| File | Purpose |
| --- | --- |
| docs/PRODUCT_MAP.md | Canonical truth |
| docs/MASTER_PLAN.md | Blueprint |
| docs/ux/WIREFRAMES.md | ESW + XO wireframes |
| docs/ux/SITEMAP.md | Route catalog |
| README.md | Repo overview + drift checks |

## Complete Application Sitemap
| Route | Mode | Purpose |
| --- | --- | --- |
| /app/intake | ITEM POOL | Capture intent |
| /app/memory | ITEM POOL | Things you need |
| /app/view | VIEW | Contextual list |
| /app/trips/start | TRIP | Trip assembly |
| /app/trips/:tripId | TRIP | Shop mode |
| /app/trips/:tripId/receipts | TRIP | Receipts loop |

ASCII Tree
/
|-- /app/intake
|-- /app/memory
|-- /app/view
`-- /app/trips
    |-- /start
    `-- /:tripId
        `-- /receipts

## Open Questions / Decisions
| Question | Owner | Status |
| --- | --- | --- |
| Group routes timing | Team | Open |
| Budget vs priority precedence | Team | Open |
| Store eligibility defaults | Team | Open |

## Sources
- Placeholder (add research links)

## ESW + XO References
- Wireframes: docs/ux/WIREFRAMES.md
- Sitemap: docs/ux/SITEMAP.md
- Canonical truth: docs/PRODUCT_MAP.md
