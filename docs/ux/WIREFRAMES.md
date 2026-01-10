# WIREFRAMES

Derived from `docs/PRODUCT_MAP.md`. Do not define new product behavior here.

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

## Route Catalog
| Route | Mode | Purpose | Core actions | ESW section | XO section |
| --- | --- | --- | --- | --- | --- |
| /app/intake | ITEM POOL | Capture intent + tags | Start AID -> Add Item | ESW: Intake | XO: Stepwise Intake |
| /app/memory | ITEM POOL | Persistent item memory | Edit tags, start view/trip | ESW: Memory | XO: Workspace Shell |
| /app/view | VIEW | Generated views | Filter, budget, start trip | ESW: View | XO: Workspace + Over-Budget |
| /app/trips/start | TRIP | Trip assembly | Select store, generate | ESW: Trip Start | XO: Store Zone Map |
| /app/trips/:tripId | TRIP | Shop mode | Confirm/Skip | ESW: Trip | XO: none |
| /app/trips/:tripId/receipts | TRIP | Receipt loop | Match, update prices | ESW: Receipts | XO: none |

--------------------------------------------------------------------------------
ESW: INTAKE | MODE: ITEM POOL
--------------------------------------------------------------------------------
/app/intake
+------------------------------------------------------------------------------+
| HEADER: Intake | What do you need?                                           |
+------------------------------------------------------------------------------+
| ITEM LOCATION: ITEM POOL                                                     |
|                                                                              |
| INPUT:                                                                       |
| [__________________________________________]                                |
|                                                                              |
| PROMPTS (TAGGING):                                                           |
| [Store?] [Urgent?] [For who?] [Budget impact?]                               |
|                                                                              |
| PREVIEW:                                                                     |
| Items: (empty)                                                               |
|                                                                              |
| ACTION: [Start AID] -> /app/intake (AID)                                     |
+------------------------------------------------------------------------------+

XO: Stepwise Intake + Live Preview
+------------------------------------------------------------------------------+
| XO OVERLAY: Stepwise Intake + Live Preview                                   |
+------------------------------------------------------------------------------+
| Binds to: /app/intake | State: TYPING | Action: Start AID                    |
| Shell: stepper dots + preview scaffold forming                               |
+------------------------------------------------------------------------------+

--------------------------------------------------------------------------------
ESW: AID | MODE: ITEM POOL
--------------------------------------------------------------------------------
/app/intake (AID)
+------------------------------------------------------------------------------+
| HEADER: AID | Assisted Item Definition                                       |
+------------------------------------------------------------------------------+
| ITEM LOCATION: ITEM POOL                                                     |
|                                                                              |
| LEFT: TIPS/PROMPTS       | CENTER: PRIMARY IMAGE      | RIGHT: INTERPRETATION |
| - Brand helps            | +----------------------+   | Item: [Standing Desk] |
| - Size matters           | |     PRIMARY IMAGE     |   | Category: [Furniture] |
| - Any brand?             | |     (placeholder)     |   | Tags: [Store eligible] |
| Prompt: Add size? [S][M] | +----------------------+   | Price range: [$-$$]    |
|                          | Variations (4-6):           |                       |
|                          | +----+ +----+ +----+        |                       |
|                          | |IMG| |IMG| |IMG|    ...    |                       |
|                                                                              |
| ACTION: [Add Item] -> /app/memory | What do you want to add next?             |
+------------------------------------------------------------------------------+

XO: none (default ESW)

--------------------------------------------------------------------------------
ESW: MEMORY | MODE: ITEM POOL
--------------------------------------------------------------------------------
/app/memory
+------------------------------------------------------------------------------+
| HEADER: Item Pool | Things you need                                          |
+------------------------------------------------------------------------------+
| ITEM LOCATION: ITEM POOL                                                     |
|                                                                              |
| ITEMS:                                                                       |
| [ ] Desk Lamp  | Tags: Nice, Office | Price Memory: $89                      |
| [>] Chair      | Tags: Must, Office | Price Memory: $450                     |
| [ ] Diapers    | Tags: Must, Kids   | Price Memory: $28                      |
|                                                                              |
| ACTION: [Start View] -> /app/view | [Start Trip] -> /app/trips/start          |
+------------------------------------------------------------------------------+

XO: Workspace Shell (Consolidated Panels)
+------------------------------------------------------------------------------+
| XO OVERLAY: Workspace Shell (Consolidated Panels)                            |
+------------------------------------------------------------------------------+
| Binds to: /app/memory | State: DEFAULT | Action: Start View/Trip             |
| Shell: unified workspace frame + consolidated panels                          |
+------------------------------------------------------------------------------+

--------------------------------------------------------------------------------
ESW: VIEW | MODE: VIEW
--------------------------------------------------------------------------------
/app/view
+------------------------------------------------------------------------------+
| HEADER: View | Budget Impact                                                 |
+------------------------------------------------------------------------------+
| ITEM LOCATION: VIEW                                                          |
|                                                                              |
| QUERY: Filters [Budget][Store][Priority][Tags]                               |
| RESULTS: HIGH: Desk $749 | MED: Chair $450 | LOW: Lamp $89                    |
|                                                                              |
| ACTION: [Start Trip] -> /app/trips/start                                     |
+------------------------------------------------------------------------------+

XO: Workspace Shell (Consolidated Panels)
+------------------------------------------------------------------------------+
| XO OVERLAY: Workspace Shell (Consolidated Panels)                            |
+------------------------------------------------------------------------------+
| Binds to: /app/view | State: DEFAULT | Action: Start Trip                    |
| Shell: consolidated panels + ambient workspace frame                          |
+------------------------------------------------------------------------------+

XO: Over-Budget Balance
+------------------------------------------------------------------------------+
| XO OVERLAY: Over-Budget Balance                                              |
+------------------------------------------------------------------------------+
| Binds to: /app/view | State: OVER_BUDGET | Action: Trim/Reprioritize          |
| Shell: ambient glow or balance scale visualization                            |
+------------------------------------------------------------------------------+

--------------------------------------------------------------------------------
ESW: TRIP START | MODE: TRIP
--------------------------------------------------------------------------------
/app/trips/start
+------------------------------------------------------------------------------+
| HEADER: Trip Start | Select Store                                             |
+------------------------------------------------------------------------------+
| ITEM LOCATION: TRIP (GENERATED FROM POOL)                                    |
|                                                                              |
| Stores: [Kroger] [Target] [HD]                                               |
| Eligible items (cross-pollinated): Lamp, Plant, Cables                        |
|                                                                              |
| ACTION: [Generate Trip] -> /app/trips/:tripId                                |
+------------------------------------------------------------------------------+

XO: Store Planning Zone Map
+------------------------------------------------------------------------------+
| XO OVERLAY: Store Planning Zone Map                                          |
+------------------------------------------------------------------------------+
| Binds to: /app/trips/start | State: SELECT_STORE | Action: Generate Trip      |
| Shell: drag items from unassigned cloud into store zones                      |
+------------------------------------------------------------------------------+

--------------------------------------------------------------------------------
ESW: TRIP | MODE: TRIP
--------------------------------------------------------------------------------
/app/trips/:tripId
+------------------------------------------------------------------------------+
| HEADER: Trip | Offline | 3 of 8                                              |
+------------------------------------------------------------------------------+
| ITEM LOCATION: TRIP                                                          |
|                                                                              |
| [ITEM IMAGE] Ergonomic Chair $450                                            |
| Status: [ ] pending  [x] confirmed  [>] active                               |
|                                                                              |
| ACTIONS: Swipe Left Skip | Swipe Right Got it | Swipe Down Cant find          |
+------------------------------------------------------------------------------+

XO: none (default ESW)

--------------------------------------------------------------------------------
ESW: RECEIPTS | MODE: TRIP
--------------------------------------------------------------------------------
/app/trips/:tripId/receipts
+------------------------------------------------------------------------------+
| HEADER: Receipts | Match Items                                               |
+------------------------------------------------------------------------------+
| ITEM LOCATION: TRIP -> ITEM POOL                                             |
|                                                                              |
| RECEIPT: STANDDESK-60 $749  ---->  Standing Desk [x]                         |
| RECEIPT: CABLE-MANAGE $24  ---->  Cable Mgmt [x]                             |
|                                                                              |
| ACTION: [Done Matching] -> Update price memory                               |
+------------------------------------------------------------------------------+

XO: none (default ESW)
