# Shopping List Aggregator

| Repo Overview | Purpose | Authority |
| --- | --- | --- |
| [docs/PRODUCT_MAP.md](docs/PRODUCT_MAP.md) | Canonical system truth (item-first graph) | Source of truth |
| [docs/MASTER_PLAN.md](docs/MASTER_PLAN.md) | End-to-end behavior (phased journey) | Source of truth |

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

Derived artifacts live in `/ux` and are read-only reflections of PRODUCT_MAP.
