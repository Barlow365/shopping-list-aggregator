# PRODUCT MAP

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
THE ONE LIST SYSTEM | PRODUCT MAP (ITEM POOL CORE)
Capture | Decide | Shop | Close Loop | Scale Up
================================================================================

| CORE OBJECT: ITEM POOL
|  Items persist across time.
|  Items are tagged with store eligibility, priority, budget relevance, and who/what its for.
|  Items are never trapped in store carts or store-specific lists.
|
| INTAKE MODE (DEFAULT CAPTURE)
|  First-run and empty states start with: What do you want to buy?
|  System parses items + store mentions.
|  System asks one follow-up at a time (need now vs later, budget, for who).
|  Ends with: Start Trip | View Pool | Invite
|  Assisted Item Definition (AID) is shown before final Add
|  AID uses 3-panel clarifier: tips | primary match | interpretation
|
| QUERY-DRIVEN VIEWS (GENERATED)
|  Show me everything I added (default)
|  Show me by budget impact
|  Show me Must items only
|  Show me items for Kids / Dinner / Remodel
|  Show me what to buy at Kroger
|
| TRIP GENERATION (STORE/TIME BOUND)
|  Trip Start: Im going to Kroger
|  System generates a store-specific trip list from eligible pool items.
|  Cross-pollination is required (do not trap items to prior stores).
|  Trip Start can invoke AID to refine ambiguous items before adding to trip
|
| TRIP MODE (SHOP)
|  Store checklist, offline checkoff, purchased state
|  Trip is persisted for receipts/history
|
| RECEIPTS + PRICE MEMORY (CLOSE LOOP)
|  Receipt attached to Trip
|  Manual entry updates item price memory
|  Price memory feeds back into the pool
