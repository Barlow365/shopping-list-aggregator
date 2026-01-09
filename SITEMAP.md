================================================================================
THE ONE LIST SYSTEM
Capture  Decide  Shop  Close Loop  Scale Up
================================================================================

# Shopping List Aggregator - Full Sitemap + Route Map (MVP)

Concept: the list is the home screen. Every advanced feature is a mode or layer on the same list.

This sitemap includes every feature with phased execution:
- PHASE 0 = scaffolding, stubs, UI placeholders, feature flags
- PHASE 1 = manual workflow implementation
- PHASE 2 = automation, intelligence, integrations

---

## Marketing / Public Pages

```
/               Home (value prop: one list, many stores, budget-aware)
/how-it-works   3 steps: Capture -> Decide -> Shop
/features       Feature cards: Shared, Budget, Multi-store, Recipes, Voice, Receipt, Delivery, Recurring, Pooling
/pricing        Placeholder: Free during MVP, Pro later
/privacy        Privacy policy
/terms          Terms of service
```

---

## App Pages (Core Product)

### Auth + Onboarding

```
/app            Redirect to /login if logged out, /app/home if logged in
/login          Login
/signup         Sign up
/onboarding     Create or join first group
                Create first list
                Set budget (optional, encouraged)
                Invite members (link)
```

---

## ASCII Overview (Public + Entry)

```
PUBLIC / MARKETING                  AUTH / ENTRY

/                            --->   /login  /signup
/how-it-works                       /onboarding
/features                           (create group + list)
/pricing (placeholder)
/privacy /terms
```

## ASCII Overview (App Home / Hub)

```
/app/home
  - Active List Snapshot (budget, total, stores used, must items)
  - Recent Activity (who added what)
  - Quick Actions: Add Items | Shop Mode | Budget | Invite
```

## ASCII Overview (Groups + List Workspace)

```
GROUPS / HOUSEHOLDS                  THE LIST WORKSPACE (THE PRODUCT)

/app/groups                    --->  /app/lists/:listId
/app/groups/:groupId                  (tabs below)
  - members + permissions
  - 1 ACTIVE list selector
  - archived lists
```

## ASCII Overview (List Workspace Tabs - All Features)

```
LIST WORKSPACE TABS (ALL FEATURES)
Legend:  Active now    Manual/Placeholder    Stub/Coming Soon (wired)

CAPTURE                         DECIDE

  Items                       Budget
 /items                       /budget
 - quick add                  - running total
 - split by commas            - over budget banner
 - qty/notes                  - remove nice-to-have
 - add-by tracking            - must-only filter

  Stores                      Recipes
 /stores                      /recipes
 - assign store later         - manual recipe card
 - unassigned bucket          - add ingredients
 - filter by store            - cost/serving (stub)

  Voice
 /voice
 - UI placeholder
 - endpoint stub

SHOP                            CLOSE THE LOOP

  Shop Mode                    Receipts
 /shop                         /receipts
 - choose store                - upload photo
 - big checklist               - manual match/prices
 - offline checkoff            - update last paid

  Delivery                     History
 /delivery                     /history
 - export list now             - past totals
 - integrations stub           - price memory table

SCALE UP

  Recurring                    Pooling / Splits             Approvals / Caps
 /recurring                    /pooling                      /controls
 - rules stub                  - pool money stub             - expensive item gate
 - forecast stub               - assign buyers stub          - per-person caps
```

### Dashboard (Simple)

```
/app/home       Active list summary (primary entry point)
                Quick stats: total vs budget, stores used, must items
                Recent activity (who added what)
                Shortcuts: Invite, Archive, New list (guarded by one-active rule)
```

### Groups (Households / Teams / Neighbors)

```
/app/groups             Groups index
/app/groups/:groupId    Group overview
                        Members
                        Permissions (stub)
                        Active list selector (one active list rule)
                        Archived lists (read-only)
```

### Lists Hub (Core)

```
/app/lists              Optional index (if multiple archived lists exist)
/app/lists/:listId      List home with tabs/modes
                        Items | Store View | Budget | Shop Mode | Receipts | History | Voice | Recipes | Recurring | Delivery | Pooling | Controls | Upgrades
```

---

## Product Story Sitemap (Hub and Spoke)

Name the sections as a product story so the structure is memorable.

### Capture

**/app/lists/:listId/items**
- Purpose: capture and organize items inside the list
- Key UI blocks: quick entry, item cards, inline edit, priority, price estimate
- Features covered: shared list, item fields, priorities, store assignment (deferred), tags

**/app/lists/:listId/voice**
- Purpose: capture items by voice (phased)
- Key UI blocks: voice prompt, fallback paste input
- Phase behavior:
  - Phase 0: UI placeholder + feature flag
  - Phase 1: paste text input with auto-split
  - Phase 2: voice capture + NLP

### Decide

**/app/lists/:listId/budget**
- Purpose: make tradeoffs based on budget
- Key UI blocks: budget set/edit, running total, over-budget banner, remove nice-to-have
- Features covered: budget awareness, tradeoffs, price memory

**/app/lists/:listId/recipes**
- Purpose: plan meals and add ingredients
- Key UI blocks: recipe card list, add recipe, add ingredients to list
- Phase behavior:
  - Phase 0: stub page
  - Phase 1: manual recipe cards
  - Phase 2: URL import + auto ingredients

### Shop

**/app/lists/:listId/stores**
- Purpose: multi-store planning and assignment
- Key UI blocks: store chips, unassigned items, bulk assign
- Features covered: store filter, store assignment timing, custom store

**/app/lists/:listId/shop**
- Purpose: in-store execution
- Key UI blocks: full-screen checklist, large tap targets, cart vs purchased
- Features covered: shopping mode, store-based shopping, offline support

**/app/lists/:listId/delivery**
- Purpose: handoff for delivery (phased)
- Key UI blocks: export list actions, integration placeholders
- Phase behavior:
  - Phase 0: stub page
  - Phase 1: export list
  - Phase 2: partner integrations

### Close the Loop

**/app/lists/:listId/receipts**
- Purpose: reconcile spend and update price memory
- Key UI blocks: upload receipt, manual match, actual prices, budget vs actual
- Features covered: receipt capture, manual reconciliation, price learning

**/app/lists/:listId/history**
- Purpose: view list activity and learning trail
- Key UI blocks: item timeline, price changes
- Phase behavior:
  - Phase 0: stub page
  - Phase 1: basic event log
  - Phase 2: insights

### Scale Up

**/app/lists/:listId/recurring**
- Purpose: repeat items over time (phased)
- Key UI blocks: recurring rules, next run preview
- Phase behavior:
  - Phase 0: stub page
  - Phase 1: manual recurrence
  - Phase 2: auto add + forecasting

**/app/lists/:listId/pooling**
- Purpose: group buying and split costs (phased)
- Key UI blocks: pool setup, assign buyers, split view
- Phase behavior:
  - Phase 0: stub page
  - Phase 1: manual assignment
  - Phase 2: split payments + approvals

**/app/lists/:listId/controls**
- Purpose: approvals and spending caps (phased)
- Key UI blocks: approval rules, cap settings, pending approvals list
- Phase behavior:
  - Phase 0: stub page
  - Phase 1: manual approval tagging
  - Phase 2: automated gating + approvals

**/app/lists/:listId/upgrades**
- Purpose: surface future capabilities without blocking core use
- Key UI blocks: feature toggles, learn-more panels
- Phase behavior:
  - Phase 0: flags only
  - Phase 1: manual workflows enabled
  - Phase 2: integrations enabled

---

## App Utilities

```
/app/settings    Profile, sessions, list members
/app/help        Feedback form / support
/system          Empty states, offline status, sync conflicts, error states
```

---

## Notes for Engineering and Product

- List is the home surface; every route returns to /app/lists/:listId.
- Store assignment is late-binding and never required at item creation.
- Items originate only from user input in the list across all phases.
- Stubs must be clearly labeled and not block core flows.
