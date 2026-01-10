# Shopping List Aggregator

| Repo Overview | Purpose | Authority |
| --- | --- | --- |
| [docs/PRODUCT_MAP.md](docs/PRODUCT_MAP.md) | Canonical system truth (item-first graph) | Source of truth |
| [docs/MASTER_PLAN.md](docs/MASTER_PLAN.md) | End-to-end behavior (phased journey) | Source of truth |
| [docs/ux/WIREFRAMES.md](docs/ux/WIREFRAMES.md) | ESW wireframes (derived) | Derived |
| [docs/ux/SITEMAP.md](docs/ux/SITEMAP.md) | Route map (derived) | Derived |

| Core Terms | Meaning |
| --- | --- |
| ITEM POOL | Global memory of items (items exist before lists) |
| TAGGING | Store, urgency, budget, person, category |
| GENERATED VIEW | Contextual list assembled from tagged items |
| TRIP | Time-bound shopping execution generated from a view |

# EXECUTABLE STRUCTURAL WIREFRAMES (ESW)
Wireframe standard:
- Explicit, column-based ASCII schematics with vertical rails.
- Every wireframe includes: HEADER line, MODE indicator (ITEM POOL / VIEW / TRIP), 2-3 columns where relevant.
- Required symbols: [ ] pending, [x] confirmed, [>] active, [?] suggested.
- Every wireframe answers: item location, mode, action that moves the item forward.

Docs in this repo use a single vocabulary: ITEM POOL, TAGGING, GENERATED VIEW, TRIP.
