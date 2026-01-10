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
| ACTION: [Start AID] -> AID (modal)                                     |
+------------------------------------------------------------------------------+

XO: Stepwise Intake + Live Preview
+------------------------------------------------------------------------------+
| XO OVERLAY: Stepwise Intake + Live Preview                                   |
+------------------------------------------------------------------------------+
| Binds to: /app/intake | State: TYPING | Action: Start AID                    |
| Shell: stepper dots + preview scaffold forming                               |
+------------------------------------------------------------------------------+

XO: Onboarding Workspace Builder (Conversational)
+------------------------------------------------------------------------------+
| XO OVERLAY: Onboarding Workspace Builder                                     |
+------------------------------------------------------------------------------+
| Binds to: /app/intake | State: TYPING | Action: Start adding                 |
|                                                                              |
| Whats your first project?                                                    |
| [Office renovation________________]                                          |
|                                                                              |
| PREVIEW: Building your workspace...                                          |
|                                                                              |
|   Office renovation                                                          |
|   Budget: [$ optional_____]                                                  |
|   What do you need?                                                          |
|   [_________________________]                                                |
|                                                                              |
| [ Back ]                                                [ Start adding ]     |
+------------------------------------------------------------------------------+
| HOW IT WORKS                                                                 |
| - Ask one question at a time                                                 |
| - Each answer animates into a PREVIEW of what youre building                 |
| - You SEE your workspace taking shape as you answer                          |
| - Can start adding items immediately - no forced setup steps                 |
+------------------------------------------------------------------------------+

--------------------------------------------------------------------------------
ESW: AID | MODE: ITEM POOL
--------------------------------------------------------------------------------
STATE: AID (modal, under /app/intake)
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
| HEADER: Things you need | Memory                                          |
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

XO: Main View - Core Experience (Split-screen Intent Builder)
+------------------------------------------------------------------------------+
| XO OVERLAY: Main View - Core Experience                                      |
+------------------------------------------------------------------------------+
| THINGS YOU NEED    Office Renovation                        [$2,400 budget]   [AV]  |
| Binds to: /app/memory | State: DEFAULT | Action: Add Item                    |
| Shell: split-screen intent builder with visual refinement                    |
+------------------------------------------------------------------------------+
| AMBIENT BUDGET GLOW                                                          |
| (edge of screen glows: blue=under, amber=near limit, red=over)                |
|                                                                              |
| INTENT INPUT                         | VISUAL PREVIEW                        |
| What do you need?                     | (image morphs as you type)            |
| [a standing desk for the____]         | [Standing Desk]                       |
| [office, preferably white__]          |                                       |
| REFINERS (appear as you type):        | SIMILAR OPTIONS:                      |
| Style:  [Modern ]                     | $599  $749  $899  $450                |
| Color:  White  Black  Oak             | (tap any to select)                   |
| Price:  [$___] - [$800__]             |                                       |
| From:   [Amazon ]                     |                                       |
|                                      |                                       |
| [+ Add to memory]                       |                                       |
+------------------------------------------------------------------------------+
| YOUR ITEMS                                                    $1,847 / $2,400 |
|                                                                              |
| [desk img]           [chair img]          [monitor img]                       |
| Ergonomic Chair      27" Monitor          Desk Lamp                           |
| $450  Amazon         $350  Best Buy       $89  IKEA                           |
| MUST                 SHOULD              NICE                                |
|                                                                              |
| (drag cards to reorder priority  top = most important)                       |
+------------------------------------------------------------------------------+
| HOW IT WORKS                                                                 |
| - LEFT: Type naturally what you need - refiners appear contextually          |
| - RIGHT: Visual preview updates as you describe - see options below          |
| - ITEMS: Visual cards with images, not checkboxes                            |
| - BUDGET: Ambient glow at edges - you FEEL the budget without numbers        |
| - PRIORITY: Drag cards up/down to reorder importance                         |
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
| THINGS YOU NEED    Office Renovation                                          [AV]  |
| Binds to: /app/view | State: OVER_BUDGET | Action: Trim/Reprioritize          |
| Shell: physical balance scale metaphor                                       |
+------------------------------------------------------------------------------+
| LETS BALANCE THIS                                                            |
|                                                                              |
| YOUR BUDGET                              YOUR ITEMS                          |
|                                                                              |
|   $2,400                                  $2,847 (over by $447)              |
|                                                                              |
|                               scale tips right (over budget)                 |
+------------------------------------------------------------------------------+
| DRAG ITEMS OFF TO BALANCE:                                                   |
|                                                                              |
|   Desk Lamp     Cable Mgmt     Plant         Art Print                        |
|   $89  NICE     $45  NICE      $60  NICE     $120  NICE                       |
|   (drag off)    (drag off)     (drag off)    (drag off)                       |
|                                                                              |
| MUST-HAVES (protected):                                                      |
| Standing Desk $749  Ergonomic Chair $450  Monitor $350                        |
+------------------------------------------------------------------------------+
| LATER (items you dragged off):                                               |
|                                                                              |
|   Plant   Art        (these wait for next time)                              |
|   $60     $120                                                               |
|                                                                              |
|                                           [Done Balancing]                   |
+------------------------------------------------------------------------------+
| HOW IT WORKS                                                                 |
| - Scale visualization makes over-budget TANGIBLE                             |
| - Drag items off the scale  they float to \"Later\" cloud                     |
| - Watch the scale rebalance in real-time (satisfying physics)                |
| - Must-haves are locked (anchor icon) - cant accidentally remove             |
| - \"Later\" items arent deleted - saved for next time                         |
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
| THINGS YOU NEED    Office Renovation                     STORE PLANNING       [AV] |
| Binds to: /app/trips/start | State: SELECT_STORE | Action: Generate Trip      |
| Shell: drag items from unassigned cloud into store zones                      |
+------------------------------------------------------------------------------+
| PURPOSE: Plan which stores to visit and what to get where                     |
| INNOVATIVE INTERACTION: Abstract map with draggable zones                    |
|                                                                              |
| /app/trips/start (zone map)                                                   |
| AMAZON                    BEST BUY                                          |
|        $1,199                      $350                                      |
|                                                                              |
|        Desk                     Monitor                                      |
|        Chair                                                                |
|                                                                              |
|                   UNASSIGNED                                                 |
|                    Lamp                                                      |
|                    Plant       drag these to a store                         |
|                    Cables                                                    |
|                                                                              |
|         IKEA                     + ADD                                       |
|          $0                       STORE                                      |
|         (empty)                                                            |
|                                                                              |
| DRAG items from the cloud to assign them to stores                           |
| DRAW a line between stores to plan your route                                |
+------------------------------------------------------------------------------+
| HOW IT WORKS                                                                 |
| - Stores are ZONES on a canvas, not a list                                   |
| - Unassigned items float in a cloud at center                                |
| - DRAG items from cloud to store zones                                       |
| - Each zone shows total cost                                                 |
| - Zones pulse/glow if approaching a limit                                    |
| - Draw route lines between stores to plan trip order                         |
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

XO: Shopping Mode - Swipe Flow
+------------------------------------------------------------------------------+
| XO OVERLAY: Shopping Mode - Swipe Flow                                       |
+------------------------------------------------------------------------------+
| Binds to: /app/trips/:tripId | State: SHOPPING | Action: Confirm/Skip         |
| Shell: one item at a time, swipe to progress                                |
|                                                                             |
| SHOP MODE                                     $1,247 / $2,400                |
| (progress path: done  current  remaining)           3 of 8                  |
|                                                                             |
| [CHAIR IMAGE]                                                               |
|                                                                             |
| Ergonomic Office Chair                                                      |
| $450  Amazon                                                                |
| \"The Herman Miller alternative we discussed\"                               |
|                                                                             |
| SWIPE LEFT: Skip for now        SWIPE RIGHT: Got it                         |
| SWIPE DOWN: Can't find it                                                   |
|                                                                             |
| [Exit Shop Mode]                            Running total: $450 so far       |
|                                                                             |
| Progress: 3 of 8  | Running total updates                                    |
+------------------------------------------------------------------------------+
| HOW IT WORKS                                                                 |
| - ONE item at a time - massive, focused display                              |
| - Shows image so you know what youre looking for                             |
| - SWIPE RIGHT  got it (satisfying fly-away animation)                        |
| - SWIPE LEFT  skip (comes back at end)                                       |
| - SWIPE DOWN  cant find (flags for alternative)                              |
| - Progress shown as a path youre walking                                     |
| - Running total updates with each swipe                                     |
| - No buttons to tap - pure gesture interaction                               |
+------------------------------------------------------------------------------+

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

XO: Receipts - The Matching Game
+------------------------------------------------------------------------------+
| XO OVERLAY: Receipts - The Matching Game                                     |
+------------------------------------------------------------------------------+
| THINGS YOU NEED    Office Renovation                        RECEIPTS          [AV] |
| Binds to: /app/trips/:tripId/receipts | State: MATCHING | Action: Done Matching |
| PURPOSE: Close the loop, update prices, see planned vs actual                |
| INNOVATIVE INTERACTION: Draw lines to match, visual comparison               |
+------------------------------------------------------------------------------+
| RECEIPT (from Amazon)           YOUR LIST                                    |
|                                                                              |
| ERGO-CHAIR-PRO    $449.99    Ergonomic Chair                                 |
| STANDDESK-60      $749.00    Standing Desk                                   |
| USB-HUB-7PORT      $34.99   (not on list - add?)                             |
| CABLE-MANAGE        $24.99   Cable Management                                |
|                                                                              |
| Monitor (not purchased)          Desk Lamp (not purchased)                   |
| TOTAL:            $1,258.97                                               |
|                                                                              |
| DRAW LINES to connect receipt items to your list items                       |
| Dotted lines = auto-suggested matches (tap to confirm)                       |
+------------------------------------------------------------------------------+
| COMPARISON                                                                  |
|                                                                              |
| PLANNED          ACTUAL           DIFFERENCE                                |
| $1,200           $1,258.97        +$58.97 over                               |
| (USB hub was unplanned)                                                     |
|                                                                              |
|                             [Done Matching]                                  |
+------------------------------------------------------------------------------+
| HOW IT WORKS                                                                 |
| - Receipt on LEFT, your list on RIGHT                                        |
| - DRAW LINES connecting matching items (or tap dotted lines)                 |
| - Unmatched receipt items glow - decide to add or ignore                     |
| - Visual bar comparison: planned vs actual                                   |
| - Price differences highlighted in green (saved) or red (over)               |
| - Matched prices automatically update future estimates                       |
+------------------------------------------------------------------------------+

XO: Collaboration - Shared Workspace (FUTURE)
+------------------------------------------------------------------------------+
| XO OVERLAY: Collaboration - Shared Workspace                                 |
+------------------------------------------------------------------------------+
| THINGS YOU NEED    Marketing Team                                             [AV]  |
| Route placeholder: /groups/:groupId (not implemented)                        |
| Binds to: /groups/:groupId | State: ACTIVE | Action: Approve/Reject           |
| PURPOSE: See team activity, make decisions together                          |
| INNOVATIVE INTERACTION: Avatars around a shared canvas                       |
+------------------------------------------------------------------------------+
| Sarah (viewing)                                                              |
|                                                                              |
| Alex     Jordan                                                              |
| (editing desk)                              (away)                           |
|                                                                              |
| ACTIVE PROJECT: Office Renovation                                            |
|                                                                              |
| Desk [A]   Chair      Monitor                                                |
| $749       $450       $350                                                   |
| (editing)  agreed     ?                                                      |
|                                                                              |
| ? = needs team approval                                                      |
| Tap to vote: [Approve] [Reject] [Discuss]                                    |
|                                                                              |
| Chris (you)                                                                  |
|                                                                              |
| RECENT: Alex changed Desk price to $749  2m ago                              |
|         Sarah added Monitor  1h ago                                          |
+------------------------------------------------------------------------------+
| HOW IT WORKS                                                                 |
| - Team members shown as avatars AROUND the shared canvas                     |
| - See whos viewing, whos editing what                                        |
| - Items show who added them (avatar badge)                                   |
| - Contentious items show ? need team approval                                |
| - Tap to vote: approve, reject, discuss                                      |
| - Real-time cursors show where teammates are focused                         |
+------------------------------------------------------------------------------+

XO: Empty State - The Invitation
+------------------------------------------------------------------------------+
| XO OVERLAY: Empty State - The Invitation                                     |
+------------------------------------------------------------------------------+
| THINGS YOU NEED    New Project                                                [AV] |
| Binds to: /app/memory | State: EMPTY | Action: Start AID                     |
| PURPOSE: Get started without feeling lost                                    |
| INNOVATIVE INTERACTION: The input IS the page                                |
+------------------------------------------------------------------------------+
| What do you need?                                                           |
| [____________________________________]                                      |
|                                                                              |
| Try: \"a comfortable desk chair\"                                            |
|      \"supplies for the team offsite\"                                       |
|      \"everything for the new apartment\"                                    |
|                                                                              |
| The rest of the UI appears as you add                                        |
+------------------------------------------------------------------------------+
