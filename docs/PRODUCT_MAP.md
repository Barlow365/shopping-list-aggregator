# PRODUCT MAP

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

# EXPERIENCE OVERLAYS (XO)
XO is an overlay layer attached to ESW route/state/action.
It describes the interaction shell without changing ESW logic.

ESW defines what happens. XO defines how it feels.
XO must always reference an ESW route + state + forward action.
If an experience element is not bound to ESW, it is invalid and must be removed or bound.

| Node | Inputs | Outputs | Purpose |
| --- | --- | --- | --- |
| Item Capture | Free text, store mention | Item + tags | Intent becomes item in pool |
| Item Tagging | Priority, person, category | Tagged item | Context for views/trips |
| Generated View | Tags, budget, store context | Contextual list | Assemble by situation |
| Trip Creation | View + store/time | Trip | Time-bound execution |
| Purchase + Receipts | Trip items | Receipt + price memory | Learn and update |
| Price Memory | Receipts | Updated estimates | Better future decisions |

## Store Selection (Late-Bound)
- Store selection happens at View/Trip time, not at capture.
- This prevents cart lock-in and supports cross-pollination across stores.

## Why This Prevents Cart Lock-In
- Items live in the ITEM POOL first.
- When a user goes to Target, the system surfaces eligible items from all prior intent.
- No item is trapped inside a store list.

## XO: Collaboration Canvas (FUTURE)
- Route placeholder: /app/groups/:groupId (not implemented)
- Binds to ESW route once group routes exist.
- Overlay elements: avatars, approvals, discussion pins.
