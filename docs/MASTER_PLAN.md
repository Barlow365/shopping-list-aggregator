# MASTER PLAN

# CANONICAL PRODUCT TRUTH (NO-DRIFT)
The system is ITEM-FIRST.

1) Users add ITEMS first.
2) ITEMS live in a global ITEM POOL.
3) ITEMS are tagged (store, urgency, budget, person, category).
4) LISTS are GENERATED VIEWS of items based on context.
5) LISTS only become concrete when the user is preparing to SHOP.
6) TRIPS are time-bound shopping executions generated from lists.
7) GROUP EFFICIENCY is achieved by pooling items before store commitment.

# EXECUTABLE STRUCTURAL WIREFRAMES (ESW)
Wireframe standard:
- Explicit, column-based ASCII schematics with vertical rails.
- Every wireframe includes: HEADER line, MODE indicator (ITEM POOL / VIEW / TRIP), 2-3 columns where relevant.
- Required symbols: [ ] pending, [x] confirmed, [>] active, [?] suggested.
- Every wireframe answers: item location, mode, action that moves the item forward.

## Phase 1: Item Capture
| Step | Reads | Writes | Notes |
| --- | --- | --- | --- |
| Intake prompt | None | Item | Start with "What do you need?" |
| AID | Item | Tagged item | Clarify item before committing |

## Phase 2: Item Enrichment
| Step | Reads | Writes | Notes |
| --- | --- | --- | --- |
| Tagging | Item | Tags | Store, urgency, person, budget |
| Suggestions | Tags | Updated tags | Improve context |

## Phase 3: View Generation
| Step | Reads | Writes | Notes |
| --- | --- | --- | --- |
| View query | Item Pool + tags | Generated view | Contextual list, not yet a trip |

## Phase 4: Trip Creation
| Step | Reads | Writes | Notes |
| --- | --- | --- | --- |
| Trip start | View + store | Trip | Lists become concrete here |

## Phase 5: Purchase + Receipts
| Step | Reads | Writes | Notes |
| --- | --- | --- | --- |
| Shop mode | Trip | Trip state | Offline checklist |
| Receipt | Trip | Price memory | Close loop |

## Wireframes (ESW)
See `docs/ux/WIREFRAMES.md` for derived ESW wireframes.
