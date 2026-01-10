# MASTER PLAN

# CANONICAL PRODUCT MODEL (NO-DRIFT)
This repository describes ONE system:

1) ITEM POOL is the core object.
   - Users add items I intend to buy.
   - Items persist across time and are not trapped in store carts or store-specific lists.

2) LISTS are GENERATED VIEWS of the Item Pool.
   - A list is not a separate container.
   - Lists are views created on demand based on how the user asks to see items (store, budget, priority, tag, person, etc.).

3) TRIPS are GENERATED SHOPPING VIEWS (store/time bound).
   - A Trip is created when a user says Im going to Kroger/Target/Home Depot.
   - The system generates a store-specific set of eligible items from the Item Pool.
   - Trips can be saved for receipts/history, but items still belong to the Pool.

4) STORE selection is optional at capture time.
   - Users may specify a store (from Target) OR leave unassigned.
   - Store assignment/inference can happen at Trip Start (Im going to Kroger).

5) CROSS-POLLINATION is required.
   - If the user goes to Target, show eligible items from prior intent even if previously associated with other stores.
   - Items must always be movable/eligible across stores based on availability rules.

6) INTAKE MODE is onboarding and the default capture experience.
   - First-run and empty states start with: What do you want to buy?
   - The system asks lightweight follow-ups only after the user types/speaks intent.

7) QUERY-DRIVEN VIEWS are first-class.
   - Show me by budget impact.
   - Show me Must items.
   - Show me what to buy at Kroger.
   - Show me Kids / Dinner / Remodel.
   - Viewing is driven by how the user asks, not by navigating separate lists.

If any document or wireframe conflicts with this model, it is WRONG and must be rewritten to match.

# WIREFRAME STYLE CONTRACT (NO-DRIFT)
All wireframes in this repo must follow ONE visual grammar:

- Use the existing Inkwell-style planning layout already present in WIREFRAMES.md/MASTER_PLAN.md.
- Use vertical rails, section blocks, and column layouts.
- Do NOT introduce boxed UI mockups, new ASCII art styles, or different layout conventions.
- Every wireframe must be a zoom of the same canonical system:
  ITEM POOL  QUERY VIEW  TRIP GENERATION  SHOP MODE  RECEIPTS  PRICE MEMORY

Required conventions:
- Each wireframe must include:
  - HEADER line
  - Page/Mode name
  - 2-3 column layout where relevant
  - Clear section headings (ALL CAPS)
- Symbols:
  [X] completed / purchased / confirmed
  [>] active / in-progress
  [.] planned / pending
  [?] stubbed / reserved
- Every screen must clearly indicate whether it operates on:
  (A) ITEM POOL
  (B) GENERATED VIEW
  (C) TRIP

If any wireframe is not traceable to this system, rewrite or delete it.


## System Overview

- Core object: Item Pool
- Views: generated queries
- Trips: generated store/time views
- Receipts: attach to trips and update price memory

## Key Routes

- /app/intake
- /app/pool
- /app/views
- /app/trips/start
- /app/trips/:tripId
- /app/trips/:tripId/receipts

## Wireframes (Detailed)

================================================================================
WIREFRAME LEGEND
================================================================================
| Symbols: [X] completed | [>] active | [.] planned | [?] stub

--------------------------------------------------------------------------------
ONBOARDING / INTAKE MODE (FIRST RUN) | MODE: ITEM POOL
--------------------------------------------------------------------------------
/app/intake +-----------------------------------------------+ | HEADER: Back | Intake | Step 1/3 |
||| CONVERSATION BUILDER (LEFT)         ||| LIVE PREVIEW (RIGHT) |||
||| Q: Whats your first project?        ||| PREVIEW: Workspace forming |||
||| [Office renovation_____________]    ||| Workspace: Office renovation |||
||| Q: Set a budget? [Skip] [Set]       ||| Budget: [optional]           |||
||| Q: Who is this for? [Team][Home]    ||| Members: You                 |||
||| INPUT: What do you need?            ||| Items: (empty)               |||
||| [__________________________]        |||                             |||
||| CTA: [Back] [Start adding]          |||                             |||
+---------------------------------------+
HOW IT WORKS:
- Ask one question at a time
- Each answer animates into the preview
- Start adding items immediately

--------------------------------------------------------------------------------
ASSISTED ITEM DEFINITION (AID) | MODE: ITEM POOL
--------------------------------------------------------------------------------
/app/intake (AID) +------------------------------------------+ | HEADER: Assisted Item Definition |
||| INTENT INPUT                   ||| VISUAL PREVIEW        |||
||| What do you need?              ||| (image morphs as you type) |||
||| [a standing desk for____]      ||| +---------------------+  |||
||| [office, preferably white]     ||| |     PRIMARY IMAGE   |  |||
||| REFINERS (appear as you type): ||| |     (placeholder)   |  |||
||| Style: [Modern v]              ||| +---------------------+  |||
||| Color: o White o Black o Oak   ||| SIMILAR OPTIONS:          |||
||| Price: [$___] - [$800__]       ||| +----+ +----+ +----+ +----+|||
||| From: [Any store v]            ||| |IMG| |IMG| |IMG| |IMG|   |||
||| LIVE INTERPRETATION            ||| |599| |749| |899| |450|   |||
||| Item: [Standing Desk____]      ||| +----+ +----+ +----+ +----+|||
||| Category: [Furniture____]      ||| (tap any to select)       |||
||| Attributes: [Size][Brand]      |||                            |||
||| Tags: [Store eligible: Amazon] |||                            |||
||| ACTIONS: [Add Item] [Edit] -> What do you want to add next?  |||
+------------------------------------------+

--------------------------------------------------------------------------------
MAIN VIEW - CORE EXPERIENCE | MODE: ITEM POOL
--------------------------------------------------------------------------------
/app/pool +--------------------------------------------+ | ONE LIST Office Renovation [$2,400] [AV] |
||| AMBIENT BUDGET GLOW (blue under / amber near / red over) |||
||| INTENT INPUT                  ||| VISUAL PREVIEW         |||
||| What do you need?             ||| (image morphs as you type) |||
||| [a standing desk for____]     ||| +-------------------+   |||
||| [office, preferably white]    ||| |   IMAGE PREVIEW   |   |||
||| REFINERS (appear as you type):||| +-------------------+   |||
||| Style: [Modern v]             ||| SIMILAR OPTIONS:         |||
||| Color: o White o Black o Oak  ||| +----+ +----+ +----+ +----+|||
||| Price: [$___] - [$800__]      ||| |599| |749| |899| |450|   |||
||| From: [Any store v]           ||| +----+ +----+ +----+ +----+|||
||| [+ Add to pool]               ||| (tap any to select)      |||
||| YOUR ITEMS                                  $1,847 / $2,400 |||
||| +----+ +----+ +----+ +----+                              |||
||| |IMG| |IMG| |IMG| |IMG|                                  |||
||| |Desk| |Chair| |Monitor| |Lamp|                          |||
||| |$749| |$450| |$350| |$89|                               |||
||| |MUST| |SHOULD| |MUST| |NICE|                            |||
||| (drag cards to reorder priority - top = most important)  |||
+--------------------------------------------+

--------------------------------------------------------------------------------
QUERY VIEW BUILDER | MODE: GENERATED VIEW
--------------------------------------------------------------------------------
/app/views +--------------------------------------------+ | HEADER: Views | Create | Saved: Budget, Must, Kroger |
||| QUERY INPUT (LEFT)           ||| LIVE RESULTS (RIGHT) |||
||| Show me: [budget impact]     ||| HIGH IMPACT          |||
||| Filters: [Store] [Priority]  ||| - Standing Desk $749 |||
||| Tags: [Kids][Office][Remodel]||| - Ergonomic Chair $450|||
||| [Save View] [Start Trip]     ||| MEDIUM IMPACT        |||
|||                              ||| - Monitor $350       |||
|||                              ||| LOW IMPACT           |||
|||                              ||| - Desk Lamp $89      |||
+--------------------------------------------+

--------------------------------------------------------------------------------
OVER BUDGET - BALANCE SCALE | MODE: GENERATED VIEW
--------------------------------------------------------------------------------
/app/views/:viewId/budget +--------------------------------+ | HEADER: Lets Balance | Over by $447 |
||| YOUR BUDGET                 ||| YOUR ITEMS            |||
||| +-----------+               ||| +---------------+     |||
||| | $2,400    |               ||| | $2,847 (over) |     |||
||| +-----------+               ||| +---------------+     |||
||| (scale tips right when over budget)                   |||
||| DRAG ITEMS OFF TO BALANCE                              |||
||| +----+ +----+ +----+ +----+                           |||
||| |Lamp| |Cables| |Plant| |Art|                         |||
||| |$89 | |$45  | |$60 | |$120|                         |||
||| |NICE| |NICE | |NICE| |NICE|                         |||
||| +----+ +----+ +----+ +----+                           |||
||| MUST-HAVES: Desk $749 | Chair $450 | Monitor $350      |||
||| LATER: Plant $60 | Art $120                              |||
||| [Done Balancing]                                        |||
+--------------------------------+

--------------------------------------------------------------------------------
TRIP START - STORE PLANNING | MODE: TRIP (GENERATION)
--------------------------------------------------------------------------------
/app/trips/start +-----------------------------------------+ | HEADER: Start Trip | Choose Store |
||| STORE ZONES (CANVAS)                                  |||
||| +--------+    +--------+                              |||
||| | AMAZON |    | BEST BUY|                             |||
||| | $1,199 |    | $350   |                             |||
||| | Desk   |    | Monitor|                             |||
||| | Chair  |    |        |                             |||
||| +--------+    +--------+                              |||
||| UNASSIGNED (drag to a store): Lamp | Plant | Cables    |||
||| +--------+    +--------+                              |||
||| | IKEA   |    | + ADD  |                              |||
||| | $0     |    | STORE  |                              |||
||| | (empty)|    |        |                              |||
||| +--------+    +--------+                              |||
||| Actions: [Generate Trip] [Draw Route]                 |||
+-----------------------------------------+

--------------------------------------------------------------------------------
SHOP MODE - SWIPE FLOW | MODE: TRIP
--------------------------------------------------------------------------------
/app/trips/:tripId +--------------------------------------+ | HEADER: Trip | Amazon | $1,247 / $2,400 | 3 of 8 |
||| [ITEM IMAGE]                                         |||
||| Ergonomic Office Chair                               |||
||| $450  Amazon                                         |||
||| "The Herman Miller alternative"                      |||
||| SWIPE LEFT: Skip  SWIPE RIGHT: Got it  SWIPE DOWN: Cant find |||
||| [Exit Shop Mode]  Running total: $450 so far          |||
+--------------------------------------+

--------------------------------------------------------------------------------
RECEIPTS - MATCHING GAME | MODE: TRIP
--------------------------------------------------------------------------------
/app/trips/:tripId/receipts +-----------------------------+ | HEADER: Trip Receipts | Amazon |
||| RECEIPT (LEFT)              ||| YOUR ITEMS (RIGHT)    |||
||| ERGO-CHAIR-PRO $449.99 ----> ||| Ergonomic Chair [X]   |||
||| STANDDESK-60   $749.00 ----> ||| Standing Desk   [X]   |||
||| USB-HUB-7PORT   $34.99 ----> ||| (not on list - add?)  |||
||| CABLE-MANAGE    $24.99 ----> ||| Cable Mgmt      [X]   |||
||| TOTAL:        $1,258.97     ||| Monitor (not purchased)|||
||| DRAW LINES to connect receipt items to your items      |||
||| Dotted lines = auto-suggested matches                 |||
||| COMPARISON: Planned $1,200 | Actual $1,258.97 | +$58.97 |||
||| [Done Matching]                                       |||
+--------------------------------+

--------------------------------------------------------------------------------
COLLABORATION - SHARED WORKSPACE | MODE: ITEM POOL
--------------------------------------------------------------------------------
/app/groups/:groupId +------------------------------------+ | HEADER: Marketing Team | Active View |
||| AVATARS AROUND SHARED CANVAS                         |||
||| Alex (editing Desk)  Jordan (away)  Sarah (viewing)  |||
||| ACTIVE ITEMS                                        |||
||| +----+ +----+ +----+                                |||
||| |Desk| |Chair| |Monitor|                            |||
||| |$749| |$450| |$350|                                |||
||| |edit| |agree| | ?  |                               |||
||| +----+ +----+ +----+                                |||
||| Needs approval: [Approve] [Reject] [Discuss]        |||
||| Recent: Alex changed Desk | Sarah added Monitor     |||
+------------------------------------+

--------------------------------------------------------------------------------
EMPTY STATE - INPUT IS THE PAGE | MODE: ITEM POOL
--------------------------------------------------------------------------------
/app/pool (empty) +---------------------------------------+ | HEADER: Item Pool | New Project |
||| What do you need?                                   |||
||| [____________________________________]              |||
||| Try: "a comfortable desk chair"                     |||
|||      "supplies for the team offsite"                |||
|||      "everything for the new apartment"             |||
||| The rest of the UI appears as you add               |||
+---------------------------------------+
