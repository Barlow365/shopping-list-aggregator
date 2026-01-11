# Shopping List Aggregator

| Doc | Purpose | Authority |
| --- | --- | --- |
| [docs/PRODUCT_MAP.md](docs/PRODUCT_MAP.md) | Canonical system truth (entities, modes, state machine) | Truth |
| [docs/MASTER_PLAN.md](docs/MASTER_PLAN.md) | Product blueprint (exec summary, architecture, milestones) | Truth |
| [docs/ux/WIREFRAMES.md](docs/ux/WIREFRAMES.md) | ESW + XO route blocks (derived) | Derived |
| [docs/ux/SITEMAP.md](docs/ux/SITEMAP.md) | Route catalog (derived) | Derived |

| Spot-check Q&A (Drift) | Answer |
| --- | --- |
| Does any doc contradict item-first tagging then generated lists? | NO |
| Does any XO exist without route/state binding? | NO |
| Does MASTER_PLAN contain wireframe dumps? | NO |
| Does every route in SITEMAP have ESW wireframe? | YES |

| Before Coding Checklist | Status |
| --- | --- |
| PRODUCT_MAP defines entities, modes, state machine | Required |
| MASTER_PLAN contains blueprint sections (no wireframes) | Required |
| WIREFRAMES contain ESW + XO bindings | Required |
| SITEMAP routes align to PRODUCT_MAP | Required |

| System Model | Description | Persisted? | User-facing label |
| --- | --- | --- | --- |
| Item | Intended purchase captured as intent | Yes | Thing you need |
| Item Pool | Internal memory storage for items | Yes | Things you need |
| Tag | Store, urgency, budget, person, category | Yes | Tags/filters |
| Generated View | Contextual list from tags | No (generated) | View |
| Trip | Time-bound shopping execution | Yes | Trip |
| Receipt | Purchases recorded for a trip | Yes | Receipt |
| Price Memory | Last paid price per item | Yes | Price memory |

ESW defines what happens. XO defines how it feels.
XO must always reference an ESW route + state + forward action.
If an experience element is not bound to ESW, it is invalid and must be removed or bound.
