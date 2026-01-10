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
