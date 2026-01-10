# MVP SPEC

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


## MVP Goal

Prove that people trust the system with money decisions AND that generated trips are useful.

## MVP Rules

- Item Pool is the core object.
- Lists are generated views; Trips are generated store/time views.
- Store selection is optional at capture and inferred at Trip Start.
- Cross-pollination is required.
- One active Trip at a time; Pool persists.

## MVP Must-Haves

- Intake Mode (guided add)
- Assisted Item Definition (AID) for refining items without store lock-in
- Item Pool with tags and priorities
- Query-driven views
- Trip Start and Trip Checklist
- Receipts attached to Trip
- Price memory feedback into Pool

## Success Signals

- Users create trips from the pool in the first week
- Budget decisions are made inside generated trip views
- Receipts update price memory and influence future estimates
