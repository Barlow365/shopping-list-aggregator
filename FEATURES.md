# Shopping List Aggregator - Features Guide

**Version:** 1.1
**Last Updated:** January 2026

---

## PRODUCT_MAP Binding

This guide is a feature-level expansion of docs/PRODUCT_MAP.md.
Every feature below maps to a PRODUCT_MAP node and phase.

---

## Core Objects

| Object | Description | Key Fields |
|--------|-------------|------------|
| List | Single active list per user | name, budget, members, status |
| Item | User-entered shopping item | name, qty, store, price, priority, status |
| Store | Store lens for items | name, custom flag |
| Receipt | Post-purchase record | items, actual prices, total |
| Group | Household or team | members, permissions, active list |

---

## Feature Lenses (Aligned to PRODUCT_MAP)

### Capture

PRODUCT_MAP nodes: CAPTURE > Items, Voice, Recipes

- Items (Phase 1): quick add, comma split, qty/notes, added-by attribution
- Voice (Phase 0/1/2): stub -> paste fallback -> NLP capture
- Recipes (Phase 0/1/2): stub -> manual cards -> URL import

### Decide

PRODUCT_MAP nodes: DECIDE > Budget, Priorities

- Budget (Phase 1): manual estimates, running total, over-budget banner
- Priorities (Phase 1): Must / Should / Nice-to-have
- Actions: remove nice-to-have, show must-only

### Shop

PRODUCT_MAP nodes: SHOP > Store Assignment, Store Filters, Shop Mode

- Store assignment (Phase 1): optional and deferred
- Store filters (Phase 1): multi-store lens and unassigned bucket
- Shop mode (Phase 1): in-store checklist, offline-friendly
- Delivery (Phase 0/1/2): stub -> export -> integrations

### Close Loop

PRODUCT_MAP nodes: CLOSE LOOP > Receipts, History

- Receipts (Phase 1): upload + manual match + actual prices
- Price memory (Phase 1): last paid price per item
- History (Phase 0/1/2): stub -> activity log -> insights

### Scale Up

PRODUCT_MAP nodes: SCALE UP > Recurring, Pooling, Controls, Upgrades

- Recurring (Phase 0/1/2): stub -> manual rules -> auto add + forecasting
- Pooling (Phase 0/1/2): stub -> manual assignment -> split payments
- Controls (Phase 0/1/2): stub -> manual caps -> automated approvals
- Upgrades (Phase 0): feature flags and future toggles

---

## Behavioral Rules (Non-Negotiables)

- Items originate only from user input inside a list.
- Store assignment is optional and deferred.
- One active list per user; archives are read-only.
- Manual workflows are stable before automation is introduced.

---

## Example Flows (Text-Only)

Create list -> add items -> set budget -> shop -> enter receipt

Onboarding:
1. Create group
2. Create list
3. Set budget (optional)
4. Invite members

Shopping:
1. Filter by store
2. Enter shop mode
3. Check off items
4. Enter receipt (optional)
