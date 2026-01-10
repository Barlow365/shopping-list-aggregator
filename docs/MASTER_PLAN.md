# MASTER PLAN

# CANONICAL PRODUCT TRUTH (NO-DRIFT)
The system is ITEM-FIRST.

1) Users add ITEMS first.
2) ITEMS live in a global memory (Things you need).
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

# EXPERIENCE OVERLAYS (XO)
XO is an overlay layer attached to ESW route/state/action.
It describes the interaction shell without changing ESW logic.

ESW defines what happens. XO defines how it feels.
XO must always reference an ESW route + state + forward action.
If an experience element is not bound to ESW, it is invalid and must be removed or bound.

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
| View query | Item Pool (internal) + tags | Generated view | Contextual list, not yet a trip |

## Phase 4: Trip Creation
| Step | Reads | Writes | Notes |
| --- | --- | --- | --- |
| Trip start | View + store | Trip | Lists become concrete here |

## Phase 5: Purchase + Receipts
| Step | Reads | Writes | Notes |
| --- | --- | --- | --- |
| Shop mode | Trip | Trip state | Offline checklist |
| Receipt | Trip | Price memory | Close loop |

## Route ESW + XO Summary
| Route | ESW states | Forward actions | XO overlays |
| --- | --- | --- | --- |
| /app/intake | EMPTY -> TYPING -> TAGGING | Start AID -> Add Item | Stepwise Intake + Live Preview |
| /app/memory | DEFAULT | Start View / Start Trip | Workspace Shell |
| /app/view | DEFAULT, OVER_BUDGET | Start Trip / Trim / Reprioritize | Workspace Shell, Over-Budget Balance |
| /app/trips/start | SELECT_STORE | Generate Trip | Store Planning Zone Map |
| /app/trips/:tripId | SHOPPING | Confirm/Skip | none |
| /app/trips/:tripId/receipts | MATCHING | Update price memory | none |

## Wireframes (ESW + XO)
See `docs/ux/WIREFRAMES.md` for full ESW and XO blocks in the project wireframe style.
