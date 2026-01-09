================================================================================
THE ONE LIST SYSTEM - EXECUTION CHECKLIST
Capture  Decide  Shop  Close Loop  Scale Up
================================================================================

Purpose: deliver the full MVP with phased execution, avoiding overengineering while
being thorough and test-driven.

================================================================================
PHASE 0 - FOUNDATIONS (Scaffolding + Stubs)
================================================================================

- Routes: implement full route map from SITEMAP.md with feature flags per route
- Global layout: header, nav, list context, budget spine
- Stub pages: all phased routes render with clear labels
- States: empty, error, offline, and sync conflict states
- Testing: route smoke tests + layout snapshot
- Exit: every route renders, no broken nav, no missing stubs

================================================================================
PHASE 1 - CORE MANUAL WORKFLOWS (MVP Proof)
================================================================================

- Items: quick add, split, edit, priority, status
- Budget: manual estimates, total, over-budget banner, remove nice-to-have
- Store view: manual assignment, filter, unassigned bucket
- Shop mode: checklist, store filter, offline-friendly
- Receipts: manual match + price updates
- Groups: create group, members, invite link, one active list + archives
- Testing: end-to-end flow (create list -> add items -> budget -> shop -> receipt)
- Exit: all core flows work without automation

================================================================================
PHASE 1.5 - MANUAL EXTENDED FEATURES (Still MVP)
================================================================================

- Recipes: manual recipe cards, add ingredients to list
- Voice: paste text input fallback
- Recurring: manual recurrence rules
- Delivery: export list action
- Pooling: manual buyer assignment
- Controls: manual approvals/caps tagging
- Testing: each tab opens; manual actions save; no blocking
- Exit: extended features usable without automation

================================================================================
PHASE 2 - AUTOMATION + INTEGRATIONS (Post-Validation)
================================================================================

- Receipt OCR: auto extraction + reconciliation
- Voice: NLP capture + auto item creation
- Recipes: URL import + auto ingredient mapping
- Delivery: partner integrations (Instacart/DoorDash)
- Recurring: auto add + forecasting
- Pooling: split payments + approvals
- Controls: automated gating + approvals
- Testing: integration suites + monitoring + rollback plan
- Exit: automation improves speed without breaking manual flows

================================================================================
CROSS-CUTTING GUARDRAILS (Always On)
================================================================================

- No product catalog or store database in early phases
- Store assignment is deferred and optional
- One active list per user; archives are read-only
- Budget stays visible in list surfaces
- No automation until manual workflows are stable
- Update MVP_SPEC.md, SITEMAP.md, PAGES_COMPONENTS_PLAN.md on every scope change
- No merge without smoke tests + UX copy review
