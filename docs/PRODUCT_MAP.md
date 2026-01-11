# PRODUCT MAP

# CANONICAL PRODUCT TRUTH (NO-DRIFT)
The system is ITEM-FIRST.

1) Users add ITEMS first.
2) ITEMS live in a global memory (Things you need).
3) ITEMS are tagged (store, urgency, budget, person, category).
4) LISTS are GENERATED VIEWS of items based on context.
5) LISTS only become concrete when the user is preparing to SHOP.
6) TRIPS are time-bound shopping executions generated from lists.
7) GROUP EFFICIENCY is achieved by pooling items before store commitment.

Every doc must link back here as the source of truth.

# EXECUTABLE STRUCTURAL WIREFRAMES (ESW)
ESW defines what happens.

# EXPERIENCE OVERLAYS (XO)
XO defines how it feels and must bind to ESW route + state + action.

## Core Objects
| Object | Description | Persisted | User-facing label |
| --- | --- | --- | --- |
| Item | Intended purchase | Yes | Thing you need |
| Tag | Store, urgency, budget, person, category | Yes | Tags/filters |
| Generated View | Contextual list from tags | No | View |
| Trip | Store/time purchase execution | Yes | Trip |
| Receipt | Purchases recorded for a trip | Yes | Receipt |
| Price Memory | Last paid prices | Yes | Price memory |
| Item Pool (internal) | Memory storage | Yes | Things you need |

## Modes
| Mode | Purpose | Primary surface |
| --- | --- | --- |
| ITEM POOL | Capture and remember items | /app/memory |
| VIEW | Contextual slice of memory | /app/view |
| TRIP | Purchase execution | /app/trips/:tripId |

## State Machine (High Level)
| State | Trigger | Next State |
| --- | --- | --- |
| Intake.Empty | User starts typing | Intake.Typing |
| Intake.Typing | Tag prompts answered | Intake.Tagged |
| Intake.Tagged | Start AID | AID.Modal |
| AID.Modal | Add Item | Memory.Default |
| Memory.Default | Start View | View.Default |
| View.Default | Over budget | View.OverBudget |
| View.Default | Start Trip | Trip.SelectStore |
| Trip.SelectStore | Generate Trip | Trip.Shopping |
| Trip.Shopping | Add Receipt | Trip.Matching |
| Trip.Matching | Done Matching | Memory.Default |

## Store Selection (Late-Bound)
- Store selection happens at View/Trip time, not at capture.
- This prevents cart lock-in and supports cross-pollination across stores.

## Why This Prevents Cart Lock-In
- Items live in memory first.
- When a user goes to Target, the system surfaces eligible items from all prior intent.
- No item is trapped inside a store list.

## XO: Collaboration Canvas (FUTURE)
- Route placeholder: /groups/:groupId (not implemented)
- Binds to ESW route once group routes exist.
- Overlay elements: avatars, approvals, discussion pins.
