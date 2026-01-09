# Shopping List Aggregator - MVP Specification

**Version:** 1.0
**Last Updated:** January 2026
**Status:** Planning

---

## MVP Goal

Prove that people want one shared list across multiple stores with budget awareness, and they will trust it with money decisions.

**Success Criteria:**
- 100+ active users within first month
- 70%+ of users accept budget-driven tradeoffs
- 50%+ of users use it across multiple store types
- Average 3+ list interactions per week per user

---

## PRODUCT_MAP Binding

This spec executes docs/PRODUCT_MAP.md.
- Every feature and page is a node or zoom of PRODUCT_MAP.
- Phases define how each PRODUCT_MAP node is delivered (stub, manual, automation).

---

## Core Hypothesis

The MVP validates:
1. **People want one shared list** - Not fragmented across apps/stores
2. **They trust it with money decisions** - Budget awareness influences purchases
3. **They accept budget-driven tradeoffs** - Will remove Nice-to-have items when over budget
4. **They'll use it across multiple store types** - Groceries + household + home improvement

**Everything else compounds from there.**

---

## MVP Features (Must-Have)

### 1. Shared List Management

| Feature | Specification |
|---------|---------------|
| **Create List** | User can create a named list (e.g., "Weekly Groceries") when no active list exists. One active list per user; prior lists can be archived (read-only). |
| **Archive List** | Archive prior lists to keep one active list; archived lists are read-only |
| **Invite Members** | Share list via link or email |
| **Add Items** | Text input: "milk, cereal, bananas" auto-splits into separate items. Items only come from user input inside a list; no product catalog or store database in MVP. |
| **Item Fields** | Name, quantity, optional store (assigned later), optional price estimate |
| **Real-time Sync** | Changes appear instantly for all list members |
| **Item Status** | Mark items as "added to cart" or "purchased" |

### 2. Budget Tracking

| Feature | Specification |
|---------|---------------|
| **Set Budget** | Per-list budget (e.g., "$150 for this week") |
| **Item Price Entry** | Manual price estimate per item |
| **Running Total** | Display: "Estimated Total: $87.50 / $150.00" |
| **Budget Warning** | When total > budget, show warning banner |
| **Last Price Memory** | Store last price paid per item (from manual entry) |

### 3. Priority Levels

| Feature | Specification |
|---------|---------------|
| **Priority Assignment** | Must / Should / Nice-to-have per item |
| **Default Priority** | Items default to "Should" |
| **Priority Filter** | "Show only Must items" view |
| **Over-Budget Helper** | Button: "Remove Nice-to-have items" |

### 4. Store Views

| Feature | Specification |
|---------|---------------|
| **Store Assignment** | Optional and deferred. Items are store-agnostic at creation and assigned later via item edit, store filter, or entering Shopping Mode. |
| **Store Filter** | "Show only [Store Name] items" |
| **Multi-Store Support** | Same list, filtered by store |
| **Store List** | Pre-populated common stores, plus "Add Custom Store" |

### 5. Shopping Mode

| Feature | Specification |
|---------|---------------|
| **Simplified View** | Full-screen checklist optimized for in-store use |
| **Large Tap Targets** | Easy to check off items with one hand |
| **Offline Support** | Works without internet connection |
| **Check-Off Action** | Tap item -> marked "purchased" -> moves to bottom |

### 6. Receipt Scanning (Basic)

| Feature | Specification |
|---------|---------------|
| **Photo Upload** | User uploads receipt photo |
| **Manual Entry** | User manually marks which items were purchased |
| **Update Prices** | Actual prices update "last price paid" |
| **Budget vs Actual** | Show "Budgeted: $150 / Actual: $163" |

Receipt entry is exploratory; low usage is not failure and informs Phase 2 automation priorities.

---

## MVP Rules (Non-Negotiables)

- Items only come from user input inside a list. No product catalog or store database in early phases.
- Store assignment is optional and deferred; items are store-agnostic at creation and assigned later via item edit, store filter, or entering Shopping Mode.
- One active list per user; prior lists can be archived (read-only).
- Receipt entry is exploratory; low usage is not failure and informs Phase 2 automation priorities.

---

## Phased Execution Within MVP

All features are included in MVP with phased execution. No feature is excluded; each ships as a stub, then manual workflow, then automation/integrations.

**Phase definitions**
- **PHASE 0** = scaffolding, stubs, UI placeholders, feature flags
- **PHASE 1** = manual workflow implementation
- **PHASE 2** = automation, intelligence, integrations

| Feature | Phase 0 (Stub) | Phase 1 (Manual) | Phase 2 (Automation) |
|---------|-----------------|------------------|----------------------|
| Shared list + members | Nav + list shell | Real-time list, invites | Advanced roles/permissions |
| Budget tracking | Budget ribbon stub | Manual estimates + warnings | Smart suggestions |
| Store views | Store filter stub | Manual store assignment | Store-level insights |
| Shopping mode | Mode shell | Checklist + offline | Smart routing |
| Receipt capture | Upload stub | Manual match + prices | OCR + auto reconciliation |
| Voice capture | Voice page stub | Paste text as fallback | Voice NLP capture |
| Recipes | Recipe page stub | Manual recipe cards | URL import + auto ingredients |
| Recurring items | Recurring page stub | Manual recurrence | Auto add + forecasting |
| Delivery | Delivery page stub | Export list for pickup | Partner integrations |
| Pooling/split costs | Pooling page stub | Manual assignment | Split payments + approvals |
| Approvals/caps | Controls page stub | Manual approvals + caps | Automated gating |

---

## User Flows

### Flow 1: Create and Share List

```
1. User opens app
2. Clicks "Create List"
3. Names list "Weekly Groceries"
4. Sets budget: $150
5. Clicks "Share" -> copies link
6. Sends link to household members via text
7. Members click link -> join list
```

### Flow 2: Add Items with Budget Tracking

```
1. User types "milk, eggs, bread" in quick entry
2. System auto-splits into 3 items
3. User adds estimate prices: $4, $3, $2
4. Running total shows: $9 / $150
5. User continues adding items
6. Running total updates in real-time
7. When total > $150, warning banner appears
8. User clicks "Show over-budget items"
9. User removes Nice-to-have items to get under budget
```

### Flow 3: In-Store Shopping

```
1. User arrives at store
2. Opens app -> clicks "Shopping Mode"
3. Filters list by store: "Kroger only"
4. Full-screen checklist appears
5. User taps items as they're added to physical cart
6. Items move to "Purchased" section
7. At checkout, user compares receipt to list
```

### Flow 4: Receipt Entry

```
1. After shopping, user clicks "Add Receipt"
2. Uploads photo of receipt
3. Manually marks which items were purchased
4. Enters actual prices from receipt
5. System updates "last price paid" for each item
6. Shows: "Budgeted: $150 / Actual: $163"
7. System learns prices for future estimates
```

---

## Technical Requirements

### Platform
- **Web app** (responsive design for mobile browsers)
- **Why web:** Fastest to ship, works on iOS + Android, no app store approval

### Performance
| Metric | Target |
|--------|--------|
| Page load time | <2 seconds |
| Real-time sync delay | <500ms |
| Offline support | Must work without internet |
| Item check-off response | <100ms |

### Data Storage
- User accounts (email + password)
- Lists (name, budget, members, status: active/archived)
- Items (name, quantity, store, price, priority, status)
- Price history (item name -> last paid price)

### Authentication
- Email + password (simple, no OAuth for MVP)
- Session persistence (stay logged in)

---

## Success Metrics

### Leading Indicators (Track Weekly)

| Metric | Target |
|--------|--------|
| **Active users** | 100+ within 30 days |
| **List creations** | 50+ lists created |
| **Items added** | 2,000+ items added |
| **Budget usage** | 70%+ of lists have budgets set |
| **Over-budget actions** | 50%+ of over-budget lists take action (remove items) |
| **Receipt uploads** | Track usage only; no minimum target (exploratory) |
| **Cross-store usage** | 50%+ of users use multiple stores |

### Lagging Indicators (Track Monthly)

| Metric | Target |
|--------|--------|
| **Retention (7-day)** | 50%+ return after 7 days |
| **Retention (30-day)** | 30%+ return after 30 days |
| **Weekly active users** | 60%+ of users active weekly |
| **Average items per list** | 15+ items per list |
| **Budget adherence** | 60%+ stay within budget after removals |

### Failure Signals (Watch For)

| Signal | Red Flag |
|--------|----------|
| **Low budget adoption** | <30% of lists use budget feature |
| **Single-store dominance** | >80% of items from one store type |
| **High abandonment** | >50% of lists never get shopped |

---

## MVP Timeline

### Phase 1: Core Development (4-6 weeks)

| Task | Time Estimate |
|------|---------------|
| User auth + account system | 1 week |
| List CRUD + real-time sync | 1 week |
| Item management | 1 week |
| Budget tracking | 1 week |
| Shopping mode | 3 days |
| Receipt upload + manual entry | 3 days |
| Testing + bug fixes | 1 week |

### Phase 2: Beta Testing (2 weeks)

| Task | Time Estimate |
|------|---------------|
| Recruit 20 beta testers | 3 days |
| Onboarding + support | Ongoing |
| Collect feedback | Daily |
| Iterate on UX issues | 1 week |

### Phase 3: Launch (1 week)

| Task | Time Estimate |
|------|---------------|
| Final polish | 2 days |
| Performance optimization | 2 days |
| Monitoring setup | 1 day |
| Public launch | 1 day |

**Total MVP Timeline:** 8-10 weeks from start to public launch

---

## MVP Non-Goals

What this MVP explicitly does NOT do:

- X Solve all household needs (focus on groceries + essentials)
- X Replace existing shopping apps (complement, don't compete yet)
- X Monetize users (free during MVP, vendor revenue comes later)
- X Scale to thousands of users (optimize for 100-500 users first)
- X Be feature-complete (minimal features to validate hypothesis)

---

## Decision Criteria for Feature Additions

Before adding any feature to MVP, ask:

1. **Does it validate a core hypothesis** (shared list, budget trust, multi-store)
2. **Can we learn without it** (if yes, defer)
3. **Is it technically simple** (if no, defer)
4. **Will users tolerate manual workarounds** (if yes, defer)

**If answer to questions 2, 3, or 4 suggests deferring, it's not in MVP.**

---

## Launch Readiness Checklist

Before declaring MVP "done":

- [ ] 5 beta users successfully complete full shopping flow
- [ ] Real-time sync works reliably across devices
- [ ] Offline mode tested on poor connection
- [ ] Budget tracking accurately reflects item totals
- [ ] Receipt entry workflow validated with 3+ users
- [ ] No critical bugs in core flows
- [ ] Performance targets met (load time, sync delay)
- [ ] Analytics tracking implemented
- [ ] Support system in place (email or chat)
- [ ] Privacy policy and terms of service published

---

## Post-MVP Learnings to Validate

After MVP launch, focus on learning:

1. **Do people actually set budgets** -> If not, why not
2. **Do they respect budget constraints** -> Do they remove items when over
3. **What stores matter most** -> Grocery-heavy or multi-category
4. **Is receipt entry too manual** -> Does it stop people from using it
5. **What's missing most** -> Voice Recipes Delivery

These learnings inform Phase 2 priorities.

---

**Remember:** The MVP proves people want this, not that we've built everything they'll ever need.
