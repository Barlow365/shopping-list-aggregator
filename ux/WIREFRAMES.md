# WIREFRAMES

# EXECUTABLE STRUCTURAL WIREFRAMES (ESW)
Wireframe standard:
- Explicit, column-based ASCII schematics with vertical rails.
- Every wireframe includes: HEADER line, MODE indicator (ITEM POOL / VIEW / TRIP), 2-3 columns where relevant.
- Required symbols: [ ] pending, [x] confirmed, [>] active, [?] suggested.
- Every wireframe answers: item location, mode, action that moves the item forward.

## Route Catalog
| Route | Mode | Purpose | Key actions | State transitions |
| --- | --- | --- | --- | --- |
| /app/intake | ITEM POOL | Capture intent + tags | Type intent, AID refine | Intake -> AID -> Pool |
| /app/memory | ITEM POOL | Browse item pool | Edit tags, start view/trip | Pool -> View/Trip |
| /app/view | VIEW | Contextual list | Filter by budget/store | View -> Trip |
| /app/trips/start | TRIP | Assemble trip | Select store, generate | Trip start -> Trip |
| /app/trips/:tripId | TRIP | Shop mode | Check off, offline | Trip -> Receipts |
| /app/trips/:tripId/receipts | TRIP | Receipt loop | Match items, update prices | Receipts -> Pool |

--------------------------------------------------------------------------------
ONBOARDING (INTAKE) | MODE: ITEM POOL
--------------------------------------------------------------------------------
/app/intake +---------------------------+ | HEADER: Intake | What do you need? |
||| ITEM LOCATION: ITEM POOL            ||| ACTION: Start AID                   |||
||| INPUT                              ||| PREVIEW                             |||
||| [__________________________]       ||| Items: (empty)                      |||
||| PROMPTS (TAGGING)                  ||| Tags appear as you answer           |||
||| [Store?] [Urgent?] [For who?]      |||                                     |||
+--------------------------------------+

--------------------------------------------------------------------------------
ASSISTED ITEM DEFINITION (AID) | MODE: ITEM POOL
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

--------------------------------------------------------------------------------
ITEM POOL (MEMORY HOME) | MODE: ITEM POOL
--------------------------------------------------------------------------------
/app/memory +------------------------------------------+ | HEADER: Item Pool |
||| ITEM LOCATION: ITEM POOL          ||| ACTION: Start View/Trip |||
||| ITEMS (POOL)                      ||| PRICE MEMORY           |||
||| [ ] Desk Lamp  [Nice] [Office]    ||| Lamp $89               |||
||| [>] Chair      [Must] [Office]    ||| Chair $450             |||
||| [ ] Diapers    [Must] [Kids]      ||| Diapers $28            |||
+--------------------------------------+

--------------------------------------------------------------------------------
GENERATED VIEW (CONTEXT) | MODE: VIEW
--------------------------------------------------------------------------------
/app/view +----------------------------------------+ | HEADER: View | Budget Impact |
||| ITEM LOCATION: VIEW            ||| ACTION: Start Trip |||
||| QUERY INPUT                    ||| RESULTS           |||
||| Filters: [Budget][Store]       ||| HIGH: Desk         |||
||| Tags: [Kids][Office]           ||| MED: Chair         |||
|||                                ||| LOW: Lamp          |||
+----------------------------------+

--------------------------------------------------------------------------------
TRIP START (ASSEMBLY) | MODE: TRIP
--------------------------------------------------------------------------------
/app/trips/start +-------------------------------------+ | HEADER: Trip Start |
||| ITEM LOCATION: TRIP (GENERATED)                   |||
||| Select store: [Kroger][Target][HD]                |||
||| Eligible items from pool (cross-pollinated)       |||
||| ACTION: [Generate Trip]                           |||
+-------------------------------------+

--------------------------------------------------------------------------------
TRIP (SHOP MODE) | MODE: TRIP
--------------------------------------------------------------------------------
/app/trips/:tripId +----------------------------------+ | HEADER: Trip | Offline |
||| ITEM LOCATION: TRIP               ||| ACTION: Confirm/Skip |||
||| [ITEM IMAGE]                      ||| [ ] pending [x] confirmed [>] active |||
||| Ergonomic Chair $450              ||| SWIPE: Left skip | Right got it       |||
+-------------------------------------+

--------------------------------------------------------------------------------
RECEIPTS (CLOSE LOOP) | MODE: TRIP
--------------------------------------------------------------------------------
/app/trips/:tripId/receipts +-------------------------+ | HEADER: Receipts |
||| ITEM LOCATION: TRIP -> ITEM POOL                 ||| ACTION: Update price memory |||
||| RECEIPT ITEMS                 ||| POOL ITEMS      |||
||| STANDDESK-60 $749  ---->      ||| Standing Desk [x]|||
||| CABLE-MANAGE $24  ---->       ||| Cable Mgmt [x]   |||
||| COMPARISON: Planned vs Actual ||| Updates price mem|||
||| Action: [Done Matching]       |||                  |||
+---------------------------------+
