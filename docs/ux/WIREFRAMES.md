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
/app/intake +-----------------------------------------------+ | HEADER: Intake | What do you need? |
||| ITEM LOCATION: ITEM POOL             ||| ACTION: Start AID            |||
||| INPUT                               ||| PREVIEW (POOL)               |||
||| [__________________________]        ||| Items: (empty)               |||
||| PROMPTS (TAGGING)                   ||| Tags appear as you answer    |||
||| [Store?] [Urgent?] [For who?]       |||                              |||
+---------------------------------------+

XO: Stepwise Intake + Live Preview
- Binds to: /app/intake + state TYPING + action Start AID
- Shell: stepper dots + preview scaffold forming as answers arrive

--------------------------------------------------------------------------------
ESW: AID | MODE: ITEM POOL
--------------------------------------------------------------------------------
/app/intake (AID)
+--------------------------------------------------------------------------------------+
| HEADER: AID | MODE: ITEM POOL                                                        |
+--------------------------------------------------------------------------------------+
| LEFT: TIPS/PROMPTS        | CENTER: PRIMARY IMAGE         | RIGHT: INTERPRETATION    |
| - Brand helps             | +--------------------------+  | Item: [Standing Desk]    |
| - Size matters            | |      PRIMARY IMAGE       |  | Category: [Furniture]    |
| - Any brand?              | |       (placeholder)      |  | Tags: [Store eligible]   |
| Prompt: Add size? [S][M]  | +--------------------------+  | Price range: [$-$$]       |
|                           | Variations (4-6):           |                              |
|                           | +----+ +----+ +----+        |                              |
|                           | |IMG| |IMG| |IMG|    ...    |                              |
| ACTION: [Add Item] -> What do you want to add next?                                    |
+--------------------------------------------------------------------------------------+

XO: none (default ESW)

--------------------------------------------------------------------------------
ESW: MEMORY | MODE: ITEM POOL
--------------------------------------------------------------------------------
/app/memory +------------------------------------------+ | HEADER: Item Pool |
||| ITEM LOCATION: ITEM POOL          ||| ACTION: Start View/Trip |||
||| ITEMS (POOL)                      ||| PRICE MEMORY           |||
||| [ ] Desk Lamp  [Nice] [Office]    ||| Lamp $89               |||
||| [>] Chair      [Must] [Office]    ||| Chair $450             |||
||| [ ] Diapers    [Must] [Kids]      ||| Diapers $28            |||
+--------------------------------------+

XO: Workspace Shell (Consolidated Panels)
- Binds to: /app/memory + state DEFAULT + action Start View/Trip
- Shell: unified workspace frame, consolidated panels

--------------------------------------------------------------------------------
ESW: VIEW | MODE: VIEW
--------------------------------------------------------------------------------
/app/view +----------------------------------------+ | HEADER: View | Budget Impact |
||| ITEM LOCATION: VIEW             ||| ACTION: Start Trip |||
||| QUERY INPUT                     ||| RESULTS           |||
||| Filters: [Budget][Store]        ||| HIGH: Desk         |||
||| Tags: [Kids][Office]            ||| MED: Chair         |||
|||                                 ||| LOW: Lamp          |||
+----------------------------------+

XO: Workspace Shell (Consolidated Panels)
- Binds to: /app/view + state DEFAULT + action Start Trip
- Shell: consolidated panels + ambient workspace frame

XO: Over-Budget Balance
- Binds to: /app/view + state OVER_BUDGET + action Trim/Reprioritize
- Shell: ambient glow or balance scale visualization

--------------------------------------------------------------------------------
ESW: TRIP START | MODE: TRIP
--------------------------------------------------------------------------------
/app/trips/start +-------------------------------------+ | HEADER: Trip Start |
||| ITEM LOCATION: TRIP (GENERATED)                   |||
||| Select store: [Kroger][Target][HD]                |||
||| Eligible items from pool (cross-pollinated)       |||
||| ACTION: [Generate Trip]                           |||
+-------------------------------------+

XO: Store Planning Zone Map
- Binds to: /app/trips/start + state SELECT_STORE + action Generate Trip
- Shell: drag items from unassigned cloud into store zones

--------------------------------------------------------------------------------
ESW: TRIP | MODE: TRIP
--------------------------------------------------------------------------------
/app/trips/:tripId +----------------------------------+ | HEADER: Trip | Offline |
||| ITEM LOCATION: TRIP               ||| ACTION: Confirm/Skip |||
||| [ITEM IMAGE]                      ||| [ ] pending [x] confirmed [>] active |||
||| Ergonomic Chair $450              ||| SWIPE: Left skip | Right got it       |||
+-------------------------------------+

XO: none (default ESW)

--------------------------------------------------------------------------------
ESW: RECEIPTS | MODE: TRIP
--------------------------------------------------------------------------------
/app/trips/:tripId/receipts +-------------------------+ | HEADER: Receipts |
||| ITEM LOCATION: TRIP -> ITEM POOL                 ||| ACTION: Update price memory |||
||| RECEIPT ITEMS                 ||| POOL ITEMS      |||
||| STANDDESK-60 $749  ---->      ||| Standing Desk [x]|||
||| CABLE-MANAGE $24  ---->       ||| Cable Mgmt [x]   |||
||| COMPARISON: Planned vs Actual ||| Updates price mem|||
||| Action: [Done Matching]       |||                  |||
+---------------------------------+

XO: none (default ESW)
