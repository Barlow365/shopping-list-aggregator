# WIREFRAMES

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


================================================================================
WIREFRAME LEGEND
================================================================================
| Symbols: [X] completed | [>] active | [.] planned | [?] stub

--------------------------------------------------------------------------------
INTAKE MODE (FIRST-RUN)  | MODE: ITEM POOL
--------------------------------------------------------------------------------
| HEADER: Back | Intake Mode | Step 1 of 3
| LAYOUT: 2-COLUMN
| PROMPT: What do you want to buy?
| INPUT: milk, eggs, diapers from Target
| FOLLOW-UP (one at a time): Need now? [Yes] [Later]
| FOLLOW-UP: For who? [House] [Kids] [Remodel]
| CTA: [Start Trip] [View Pool] [Invite]
| COLUMN A: Parsed Items
|  - Milk (store: unassigned) [.] 
|  - Eggs (store: unassigned) [.] 
|  - Diapers (store: Target)  [.] 
| COLUMN B: Tags
|  - Priority: Should
|  - Budget: Medium impact

--------------------------------------------------------------------------------
ASSISTED ITEM DEFINITION (AID)  | MODE: ITEM POOL
--------------------------------------------------------------------------------
+--------------------------------------------------------------------------------------+
| HEADER: Assisted Item Definition                                                     |
+--------------------------------------------------------------------------------------+
| LEFT PANEL (DETAILS + COACHING)              | RIGHT PANEL (IMAGE + RECOMMENDATIONS) |
+---------------------------------------------+----------------------------------------+
| INTERPRETATION (LIVE UPDATING)              | PRIMARY IMAGE                          |
| Item Name: [____________________]           | +------------------------------+       |
| Category: [______________]                  | |      PRIMARY IMAGE           |       |
| Attributes: [Size ____] [Brand ____]        | |        (placeholder)         |       |
| Tags: [Store Eligible ________] [Group ___] | +------------------------------+       |
| Price range: [$__ - $__] (soft)             | RECOMMENDATIONS (ALT PICKS)             |
|                                             | +--------+  +--------+  +--------+     |
| COACHING TIPS                               | |  IMG   |  |  IMG   |  |  IMG   |     |
| - Brand helps                               | | $24.00 |  | $27.00 |  | $29.00 |     |
| - Size matters                              | +--------+  +--------+  +--------+     |
| - Say any brand if you dont care            | +--------+  +--------+  +--------+     |
| Prompt: Add size? [S] [M] [L] [Any]         | |  IMG   |  |  IMG   |  |  IMG   |     |
|                                             | | $22.00 |  | $25.00 |  | $31.00 |     |
|                                             | +--------+  +--------+  +--------+     |
+---------------------------------------------+----------------------------------------+
| ACTIONS: [ Add Item ]  [ Edit Description ]  ->  What do you want to add next?       |
+--------------------------------------------------------------------------------------+

--------------------------------------------------------------------------------
ITEM POOL HOME  | MODE: ITEM POOL
--------------------------------------------------------------------------------
| HEADER: Pool | Filters | Profile
| LAYOUT: 2-COLUMN
| COLUMN A: ALL ITEMS (most recent first)
|  [.] Milk  tags: groceries  store-eligible: Kroger/Target
|  [.] Eggs  tags: groceries  store-eligible: Kroger
|  [.] Diapers tags: kids  store-eligible: Target
| COLUMN B: PRICE MEMORY
|  Milk $3.50 | Eggs $2.90 | Diapers $28.00
| ACTIONS: [Start Trip] [Build View] [Add Items]

--------------------------------------------------------------------------------
ADD ITEM (FROM POOL)  | MODE: ITEM POOL
--------------------------------------------------------------------------------
| TRIGGER: [Add Items]
| FLOW: Open AID -> Confirm -> Add to Pool -> Ask what to add next
|
| AID LAYOUT: 3-COLUMN (same as Intake)
| LEFT: Details/Coaching | RIGHT: Image/Recommendations
| ACTIONS: [ Add Item ] [ Edit Description ] -> After add: What do you want to add next?

--------------------------------------------------------------------------------
STORE MENTION TYPING  | MODE: ITEM POOL
--------------------------------------------------------------------------------
| INPUT: paper towels from Target
| NOTE: Store is a tag/constraint, not a lock
| AID LAYOUT: 3-COLUMN (same as Intake)
| RIGHT PANEL: Tags include store eligible Target
| ACTIONS: [ Add Item ] -> Item Pool

--------------------------------------------------------------------------------
VIEW BUILDER  | MODE: GENERATED VIEW
--------------------------------------------------------------------------------
| HEADER: Views | Create
| LAYOUT: 2-COLUMN
| QUERY: Show me by budget impact
| RESULT: High impact items first
| COLUMN A: VIEW RESULTS
|  - Diapers (High)
|  - Coffee (Medium)
| COLUMN B: VIEW OPTIONS
|  [By Store] [By Priority] [By Tag] [By Person]
| CTA: [Save View] [Start Trip]

--------------------------------------------------------------------------------
TRIP START  | MODE: TRIP (GENERATION)
--------------------------------------------------------------------------------
| HEADER: Start Trip
| LAYOUT: SINGLE COLUMN
| SELECT STORE: [Kroger] [Target] [Home Depot]
| AUTO-INFER: Based on eligible items in pool
| PREVIEW: Eligible items for Target
|  - Diapers
|  - Soap
| CTA: [Generate Trip]

--------------------------------------------------------------------------------
TRIP START REFINEMENT (AID)  | MODE: TRIP
--------------------------------------------------------------------------------
| WHEN: Trip preview shows ambiguous items
| ACTION: Tap item -> AID to refine before generating trip
| AID LAYOUT: 3-COLUMN (same as Intake)
| LEFT: Details/Coaching | RIGHT: Image/Recommendations
| ACTIONS: [Add to Trip] [Back]

--------------------------------------------------------------------------------
TRIP CHECKLIST  | MODE: TRIP
--------------------------------------------------------------------------------
| HEADER: Trip | Target | Offline Ready
| LAYOUT: 2-COLUMN
| COLUMN A: TO BUY
|  [ ] Diapers
|  [ ] Soap
| COLUMN B: PURCHASED
|  [X] Paper Towels
| ACTIONS: [Add Receipt] [End Trip]

--------------------------------------------------------------------------------
RECEIPT ENTRY  | MODE: TRIP
--------------------------------------------------------------------------------
| HEADER: Receipt | Trip: Target
| LAYOUT: SINGLE COLUMN
| UPLOAD: [Choose File] [Upload]
| LINE ITEMS
|  Diapers  Actual $27.99 [X]
|  Soap     Actual $4.49  [X]
| SUMMARY: Budgeted $40 | Actual $32.48
| ACTIONS: [Save] [Back to Trip]

--------------------------------------------------------------------------------
PRICE MEMORY SUMMARY  | MODE: ITEM POOL
--------------------------------------------------------------------------------
| HEADER: Price Memory
| LAYOUT: SINGLE COLUMN
| ITEMS
|  Milk $3.50 (last paid)
|  Eggs $2.90 (last paid)
|  Diapers $27.99 (last paid)
| ACTIONS: [Back to Pool]
