# Shopping List Aggregator

| Repo Map | Purpose | Authority |
| --- | --- | --- |
| [docs/PRODUCT_MAP.md](docs/PRODUCT_MAP.md) | Canonical system truth (item-first graph) | Truth |
| [docs/MASTER_PLAN.md](docs/MASTER_PLAN.md) | End-to-end behavior (phased journey) | Truth |
| [docs/ux/WIREFRAMES.md](docs/ux/WIREFRAMES.md) | ESW + XO route blocks | Derived |
| [docs/ux/SITEMAP.md](docs/ux/SITEMAP.md) | Route map aligned to PRODUCT_MAP | Derived |

| System Model | Description | Persisted? | User-facing label |
| --- | --- | --- | --- |
| Item | Intended purchase captured as intent | Yes | Thing you need |
| Item Pool | Global memory of items | Yes | Things you need |
| Tags | Store, urgency, budget, person, category | Yes | Tags/filters |
| Generated View | Contextual list from tags | No (generated) | View |
| Trip | Time-bound shopping execution | Yes | Trip |
| Receipt | Purchases recorded for a trip | Yes | Receipt |
| Price Memory | Last paid price per item | Yes | Price memory |

| Route Index | Mode | Core actions | ESW section | XO section |
| --- | --- | --- | --- | --- |
| /app/intake | ITEM POOL | Capture intent, tag, AID refine | docs/ux/WIREFRAMES.md (ESW: Intake) | docs/ux/WIREFRAMES.md (XO: Stepwise Intake) |
| /app/memory | ITEM POOL | Edit tags, start view/trip | docs/ux/WIREFRAMES.md (ESW: Memory) | docs/ux/WIREFRAMES.md (XO: Workspace Shell) |
| /app/view | VIEW | Filter, budget context, start trip | docs/ux/WIREFRAMES.md (ESW: View) | docs/ux/WIREFRAMES.md (XO: Workspace + Over-Budget) |
| /app/trips/start | TRIP | Select store, generate trip | docs/ux/WIREFRAMES.md (ESW: Trip Start) | docs/ux/WIREFRAMES.md (XO: Store Zone Map) |
| /app/trips/:tripId | TRIP | Shop mode, confirm items | docs/ux/WIREFRAMES.md (ESW: Trip) | docs/ux/WIREFRAMES.md (XO: none) |
| /app/trips/:tripId/receipts | TRIP | Match receipt, update prices | docs/ux/WIREFRAMES.md (ESW: Receipts) | docs/ux/WIREFRAMES.md (XO: none) |

| Visual Index | Purpose | Link |
| --- | --- | --- |
| ESW + XO Wireframes | Primary UX reference | docs/ux/WIREFRAMES.md |
| Route Map | Navigation + modes | docs/ux/SITEMAP.md |

# EXECUTABLE STRUCTURAL WIREFRAMES (ESW)
Wireframe standard:
- Explicit, column-based ASCII schematics with vertical rails.
- Every wireframe includes: HEADER line, MODE indicator (ITEM POOL / VIEW / TRIP), 2-3 columns where relevant.
- Required symbols: [ ] pending, [x] confirmed, [>] active, [?] suggested.
- Every wireframe answers: item location, mode, action that moves the item forward.

# EXPERIENCE OVERLAYS (XO)
XO is an overlay layer attached to ESW route/state/action.
It describes the interaction shell without changing ESW logic.

ESW defines what happens. XO defines how it feels.
XO must always reference an ESW route + state + forward action.
If an experience element is not bound to ESW, it is invalid and must be removed or bound.

| Spot-check Q&A | Answer |
| --- | --- |
| Does every route have ESW wireframe? | YES |
| Does every route have an optional XO overlay section? | YES |
| Does any XO exist without route/state binding? | NO |
| Does any doc contradict item-first -> tagging -> generated lists? | NO |
