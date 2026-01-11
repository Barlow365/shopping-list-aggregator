# MVP SPECIFICATION

**🔍 ZOOM: PRODUCT_MAP → MVP Rules and Constraints**

This document defines what MUST be true for MVP and what can wait for Phase 2.

## MVP Non-Negotiables

### Core Principle Enforcement
1. **Items are captured before any list exists** (item-first mandatory)
2. **Store selection is late-bound** at View/Trip time (not at capture)
3. **Trips are generated from memory** (not vice versa)
4. **Receipts update price memory** and inform future estimates
5. **No cart lock-in** (items never trapped in store-specific lists)

### Must-Have Features (Phase 1)
| Feature | Reason | Exit Criteria |
|---------|--------|---------------|
| Item capture with tags | Core loop starts here | User can add 10+ items |
| Item pool ("Things you need") | Persistent memory | Items persist after trip |
| Tags (store, urgency, budget) | Late-bound context | User can filter items |
| Budget warnings | Prevents overspend | User sees "Over budget" |
| Generate trip by store | Assembly point | User can create trip |
| Shop mode (offline) | Shopping execution | User can shop without internet |
| Receipt upload + manual match | Close the loop | User can update prices |
| Price memory per store | Learning system | System remembers Target $5, Kroger $4.50 |

### Nice-to-Have (Phase 2)
| Feature | Rationale | When to Add |
|---------|-----------|-------------|
| AID (Assisted Item Definition) | Requires external API, adds cost | After 100 users prove loop |
| OCR receipt scanning | Complex, error-prone | After manual matching validates flow |
| Voice input | Convenience, not necessity | After desktop users adopt |
| Groups / collaboration | Complex permissions | After single-user retention proves out |
| Auto-categorization | Nice but not critical | After 1000+ items to train on |

## MVP Constraints

### Technical Constraints
- **No external APIs in Phase 1** (keep costs zero)
- **Manual workflows acceptable** (prove the loop first)
- **Desktop-first** (mobile can wait)
- **Session auth only** (no OAuth complexity)

### Scope Constraints
- **Single user only** (no groups)
- **Manual price entry** (no OCR)
- **Manual tagging** (no AI suggestions)
- **Manual receipt matching** (no auto-match)

### Data Constraints
- **Max 1000 items per user** (Phase 1 limit)
- **Max 10MB receipt images** (prevent abuse)
- **Max 50 trips per user** (cleanup old trips after)

## Success Metrics

### Adoption Metrics (Week 1-4)
- **80% of users** add 5+ items in first session
- **60% of users** complete first trip
- **40% of users** upload first receipt

### Retention Metrics (Week 5-8)
- **50% of users** return within 7 days
- **30% of users** complete 2+ trips per week
- **20% of users** close loop (receipt → price memory)

### Quality Metrics
- **70% of trips** have receipts uploaded
- **80% of receipts** have 3+ items matched
- **Price memory accuracy** within 20% of actual prices

## MVP User Flows

### Flow 1: First-Time User (Happy Path)
1. Signup/login
2. Add 5-10 items with tags
3. View "Things you need" (item pool)
4. Generate trip for Target
5. Complete trip in shop mode
6. Upload receipt
7. Match items to receipt
8. See updated price memory

**Exit Criteria**: User completes this flow end-to-end without errors.

### Flow 2: Returning User
1. Login
2. View "Things you need"
3. Add 2-3 new items
4. Generate trip (system suggests stores based on eligible items)
5. Complete trip
6. Upload receipt (optional)

**Exit Criteria**: User returns 2+ times in first week.

### Flow 3: Budget-Conscious User
1. Set budget ($150 per week)
2. Add items with budget tags
3. Generate view filtered by budget
4. See "Over budget" warning
5. Adjust items (remove or defer)
6. Generate trip within budget

**Exit Criteria**: User stays within budget 70% of trips.

## MVP Anti-Patterns (What NOT to Build)

### ❌ List-First
Don't: "Create list, then add items"
Do: "Add items, then generate views"

### ❌ Store Lock-In
Don't: "This item is for Target only"
Do: "This item is eligible at Target, Kroger, Walmart"

### ❌ Cart-Based
Don't: "Add to Target cart"
Do: "Tag for Target, assemble trip later"

### ❌ Receipt Required
Don't: "You must upload receipt to complete trip"
Do: "Receipt is optional but improves estimates"

## MVP Testing Checklist

### Functional Testing
- [ ] User can signup/login
- [ ] User can add items with tags
- [ ] User can view item pool
- [ ] User can generate views by tags
- [ ] User can see budget warnings
- [ ] User can start trip
- [ ] User can complete trip offline
- [ ] User can upload receipt
- [ ] User can match receipt items
- [ ] Price memory updates after match

### Edge Case Testing
- [ ] User adds item without tags (should prompt)
- [ ] User tries to shop offline (should work)
- [ ] User uploads invalid receipt file (should reject)
- [ ] User tries to match already-matched item (should prevent)
- [ ] User exceeds budget (should warn but allow)

### Performance Testing
- [ ] App loads in <2s
- [ ] Item list renders 100+ items smoothly
- [ ] Trip generation completes in <1s
- [ ] Receipt upload completes in <5s

## MVP Launch Criteria

### Code Complete
- [ ] All routes functional
- [ ] Database schema deployed
- [ ] API endpoints tested
- [ ] Frontend tested on Chrome/Firefox/Safari
- [ ] No console errors

### Documentation Complete
- [ ] User onboarding guide
- [ ] FAQ page
- [ ] Privacy policy
- [ ] Terms of service

### Beta Ready
- [ ] 10 beta users invited
- [ ] Feedback form embedded
- [ ] Analytics tracking events
- [ ] Error monitoring active (Sentry)

### MVP Launch
- [ ] 10 beta users complete full loop
- [ ] No critical bugs reported
- [ ] Retention > 50% at 7 days
- [ ] Ready for wider beta (100 users)

## Phase 2 Triggers

| Trigger | Threshold | Action |
|---------|-----------|--------|
| Retention proves out | 60% at 7 days | Add Phase 2 features |
| Users request AID | 50%+ want it | Integrate product API |
| Users request groups | 40%+ want it | Build group system |
| Receipt matching pain | Users complain | Add OCR |
| Price memory inaccurate | >30% error | Improve matching |

---

**Next Steps**: See [EXECUTION_PLAN.md](../EXECUTION_PLAN.md) for technical phasing and [../architecture/ARCHITECTURE.md](../architecture/ARCHITECTURE.md) for implementation specs.
