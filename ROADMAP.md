# Shopping List Aggregator - Product Roadmap

**Version:** 1.0
**Last Updated:** January 2026
**Status:** Planning Phase

---

## Roadmap Overview

This roadmap is organized into 4 phases, each building on validated learnings from the previous phase.

| Phase | Focus | Timeline | Key Outcome |
|-------|-------|----------|-------------|
| **MVP** | Core validation | 8-10 weeks | Prove people want one shared list with budget awareness |
| **Phase 2** | Smart Features | 8-12 weeks | Add voice, recipes, OCR - reduce friction |
| **Phase 3** | Community | 10-14 weeks | Group buying, neighborhood pooling - network effects |
| **Phase 4** | Scale | 12+ weeks | Partnerships, monetization, enterprise features |

**Total Timeline:** 38-48 weeks (~9-12 months) from start to full launch

---

## MVP (Minimum Viable Product)

**Duration:** 8-10 weeks
**Goal:** Validate core hypotheses

### Core Features

| Feature | Hours | Status |
|---------|-------|--------|
| User Authentication | 40h | Not Started |
| List CRUD + Real-time Sync | 60h | Not Started |
| Item Management | 40h | Not Started |
| Budget Tracking | 40h | Not Started |
| Priority Levels (Must/Should/Nice) | 20h | Not Started |
| Store Views/Filters | 30h | Not Started |
| Shopping Mode | 20h | Not Started |
| Receipt Upload (Manual Entry) | 30h | Not Started |
| Price History | 20h | Not Started |
| Responsive Design | 40h | Not Started |

**Total Effort:** ~340 hours (~8-9 weeks with 1 developer)

### Success Metrics

| Metric | Target |
|--------|--------|
| Active users | 100+ within first month |
| Budget adoption | 70%+ of lists have budgets set |
| Over-budget actions | 50%+ take action when over budget |
| Cross-store usage | 50%+ use multiple stores |
| Receipt uploads | 30%+ of shopping sessions |
| Retention (7-day) | 50%+ return |

### MVP Exit Criteria

- [ ] 100 active users
- [ ] Average 3+ list interactions per week per user
- [ ] 70%+ budget adoption rate
- [ ] 50%+ users use multiple stores
- [ ] No critical bugs in core flows
- [ ] <2s page load time
- [ ] <500ms real-time sync delay

---

## Phase 2: Smart Features

**Duration:** 8-12 weeks
**Goal:** Reduce friction, increase engagement

### Features

| Feature | Effort | Priority | Rationale |
|---------|--------|----------|-----------|
| **Voice Capture** | 60h | High | Natural input, hands-free |
| **Recipe Import** | 80h | High | Meal planning integration |
| **OCR Receipt Scanning** | 60h | High | Automated price learning |
| **Aisle-by-Aisle Ordering** | 40h | Medium | Faster in-store shopping |
| **Recurring Items** | 40h | Medium | Auto-add weekly groceries |
| **Product-Level Selection** | 80h | Medium | Exact item matching |
| **Push Notifications** | 30h | Low | Reminders, updates |
| **Multiple Lists per User** | 30h | Low | Separate trips/households |

**Total Effort:** ~420 hours (~10-12 weeks with 1 developer)

### Voice Capture Details

**Input:**
> "Add chicken thighs, rice, and broccoli for dinner. Remove cookies. Add diapers size 4."

**System parses:**
- Actions: Add (3 items), Remove (1 item)
- Tags: "Dinner" applied to first 3 items
- Specific variants: "size 4" for diapers

**Technology:** OpenAI API or Claude for NLP parsing

### Recipe Import Details

**Sources:**
- Manual paste (URL or text)
- Popular recipe sites (Allrecipes, NYT Cooking)
- User's saved recipes

**Workflow:**
```
1. User pastes recipe URL
2. System extracts ingredients
3. Shows: Ingredients, estimated cost, cost per serving
4. User adjusts servings (2x, 3x)
5. User checks "Already have" for pantry items
6. Click "Add to List" → items added with "Dinner" tag
```

### OCR Receipt Scanning Details

**Providers:** Mindee (primary), AWS Textract (fallback)

**Workflow:**
```
1. User uploads receipt photo
2. OCR extracts: items, prices, total, store, date
3. Auto-matches items to list
4. Shows unmatched items (not on list)
5. User confirms matches
6. System updates last-price-paid for all items
7. Budget vs Actual shown
```

**Accuracy Target:** 90%+ item matching

### Success Metrics (Phase 2)

| Metric | Target |
|--------|--------|
| Voice capture usage | 40%+ of users try it |
| Recipe import usage | 50%+ of users import recipes |
| OCR accuracy | 90%+ item match rate |
| Recurring items adoption | 60%+ set up recurring items |
| Session length | +25% increase vs MVP |

---

## Phase 3: Community & Network Effects

**Duration:** 10-14 weeks
**Goal:** Enable group buying, build network effects

### Features

| Feature | Effort | Priority | Rationale |
|---------|--------|----------|-----------|
| **Group Buying / Neighborhood Pooling** | 120h | High | Core differentiator |
| **Cost Splitting** | 60h | High | Automated reimbursement |
| **Demand Aggregation** | 80h | Medium | Bulk pricing discounts |
| **Instacart Integration** | 80h | Medium | Digital shopping |
| **DoorDash Integration** | 60h | Medium | Multi-store delivery |
| **Walmart/Target APIs** | 100h | Medium | Product matching |
| **Approval Rules** | 40h | Low | Big-item approval |
| **Community Dashboard** | 60h | Low | Group analytics |

**Total Effort:** ~600 hours (~14-16 weeks with 1 developer)

### Group Buying Details

**Use Case:** One shopper, multiple households

**Setup:**
```
Group: "Oak Street Neighbors"
├── Household 1 (Johnsons): $50 budget
├── Household 2 (Smiths): $75 budget
└── Household 3 (Browns): $60 budget
Total Pool: $185
```

**Workflow:**
```
1. Create group "Oak Street Neighbors"
2. Invite households via link
3. Each household adds items + budget
4. System aggregates into master list
5. Shopper (Sarah) shops for all
6. Sarah uploads receipt
7. System calculates splits per household
8. Venmo/PayPal links sent for reimbursement
```

**Cost Splitting Logic:**
- Items assigned to specific households
- Shared items (bags, tax) split proportionally
- Auto-generate payment requests

### Digital Shopping Integration

**Phase 3A: Export List**
- "Open in Instacart" deep link
- Clean export format
- List remains source of truth

**Phase 3B: Product Matching**
- Pre-build cart with matched products
- Show alternatives if item unavailable
- User reviews before checkout

**Phase 3C: Full Checkout** (Future)
- Partnership with services
- Checkout within app
- Track delivery status

### Success Metrics (Phase 3)

| Metric | Target |
|--------|--------|
| Group buying adoption | 20%+ of users create groups |
| Average households per group | 3+ |
| Digital shopping usage | 40%+ use Instacart/DoorDash |
| Network effects | 30%+ of users invited by existing users |
| Revenue per user (via partnerships) | $2+ per month |

---

## Phase 4: Scale & Monetization

**Duration:** 12+ weeks
**Goal:** Monetize, scale, partnerships

### Features

| Feature | Effort | Priority | Rationale |
|---------|--------|----------|-----------|
| **Vendor Partnerships** | 80h | High | Primary revenue stream |
| **Data Insights Dashboard** | 100h | High | Sell aggregated insights |
| **Premium Features** | 60h | Medium | Optional paid tier |
| **Enterprise/B2B** | 120h | Medium | Corporate team shopping |
| **Advanced Analytics** | 80h | Medium | User behavior insights |
| **Mobile App (Native)** | 400h | Low | Better performance |
| **Internationalization** | 80h | Low | Global expansion |

**Total Effort:** ~920 hours (~23 weeks with 1 developer)

### Vendor Partnerships

**Revenue Models:**

| Model | Description | Revenue Potential |
|-------|-------------|-------------------|
| **Referral Fees** | % of sales from partner stores | $0.50-$2.00 per transaction |
| **Data Licensing** | Aggregated, anonymized insights | $10K-$50K per partner per year |
| **Fulfillment Margin** | % of digital orders | 3-5% of order value |
| **Sponsored Products** | Featured placement in lists | $1-$5 per impression |

**Target Partners:**
- Grocery chains (Kroger, Safeway, Albertsons)
- Big-box retailers (Target, Walmart)
- Home improvement (Home Depot, Lowe's)
- Delivery services (Instacart, DoorDash)
- CPG brands (P&G, Unilever)

### Premium Features (Optional)

| Feature | Free | Premium ($4.99/mo) |
|---------|------|--------------------|
| Lists | 1 | Unlimited |
| Receipt scanning | 10/month | Unlimited |
| Advanced analytics | Basic | Detailed |
| Priority support | Email | Chat + phone |
| Custom tags | 10 | Unlimited |
| Export formats | CSV | CSV, PDF, Excel |

**Conversion Target:** 5-10% of free users upgrade

### Enterprise/B2B Features

**Use Cases:**
- Office managers (snacks, supplies)
- Event planners (catering, materials)
- Small businesses (inventory)

**Features:**
- Team accounts with roles
- Department budgets
- Approval workflows
- Invoice integration
- Bulk ordering

**Pricing:** $49-$199/month per team

### Success Metrics (Phase 4)

| Metric | Target |
|--------|--------|
| Vendor partnerships | 5+ signed deals |
| Premium conversion rate | 5-10% of users |
| Revenue per user | $5+ per month (blended) |
| Enterprise customers | 10+ companies |
| MAU (Monthly Active Users) | 100,000+ |

---

## Long-Term Vision (18+ months)

### Future Possibilities

| Feature | Rationale | Effort |
|---------|-----------|--------|
| **AI Shopping Assistant** | Personalized recommendations | High |
| **Price Prediction** | Forecast future prices | Medium |
| **Meal Planning AI** | Auto-generate meal plans | High |
| **Sustainability Tracking** | Carbon footprint per item | Medium |
| **Social Features** | Share favorite lists | Low |
| **Gamification** | Rewards for staying under budget | Medium |
| **AR Shopping** | Augmented reality in-store | Very High |

**Note:** These are speculative and depend on user feedback and market trends.

---

## Resource Planning

### Team Composition

| Phase | Team Size | Roles |
|-------|-----------|-------|
| **MVP** | 2 people | 1 Full-stack Dev, 1 Designer |
| **Phase 2** | 3-4 people | 2 Devs, 1 Designer, 1 Product Manager |
| **Phase 3** | 5-7 people | 3 Devs, 1 Designer, 1 PM, 1 BD, 1 Support |
| **Phase 4** | 10+ people | 5 Devs, 2 Designers, 1 PM, 1 BD, 2 Support, Ops |

### Budget Estimate (Bootstrapped)

| Phase | Cost | Description |
|-------|------|-------------|
| **MVP** | $20-30K | 2 people × 10 weeks + hosting |
| **Phase 2** | $40-60K | 4 people × 12 weeks + tools |
| **Phase 3** | $80-120K | 6 people × 16 weeks + partnerships |
| **Phase 4** | $150-250K | 10 people × 24 weeks + scaling |

**Total Year 1:** $290K-$460K (bootstrapped with lean team)

### Technology Costs

| Service | Monthly Cost | Annual Cost |
|---------|--------------|-------------|
| Hosting (Vercel + Railway) | $50-$200 | $600-$2,400 |
| Database (PostgreSQL) | $25-$100 | $300-$1,200 |
| Redis | $10-$50 | $120-$600 |
| S3 Storage | $10-$50 | $120-$600 |
| OCR (Mindee/Textract) | $50-$500 | $600-$6,000 |
| Email (Resend/SendGrid) | $10-$50 | $120-$600 |
| Monitoring (Sentry + PostHog) | $50-$200 | $600-$2,400 |

**Total Tech Costs Year 1:** $2,460-$13,800 (scales with usage)

---

## Risk Mitigation

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| **Low adoption** | Medium | High | Extensive MVP testing, pivot if needed |
| **Competition** | High | Medium | Focus on differentiators (budget, group buying) |
| **OCR accuracy** | Medium | Medium | Manual fallback, improve over time |
| **Store API access** | High | Medium | Start with manual entry, build relationships |
| **User churn** | Medium | High | Retention features (recurring items, notifications) |
| **Security breach** | Low | High | Regular audits, bug bounty program |
| **Scaling issues** | Medium | Medium | Design for scale from day 1 |

---

## Decision Points

### After MVP (Week 10)

**Decision:** Continue to Phase 2?

**Criteria:**
- [ ] 100+ active users
- [ ] 70%+ budget adoption
- [ ] 50%+ cross-store usage
- [ ] Positive user feedback
- [ ] Clear retention signals

**If NO:** Pivot or shut down
**If YES:** Proceed to Phase 2

### After Phase 2 (Week 22)

**Decision:** Invest in community features?

**Criteria:**
- [ ] 1,000+ active users
- [ ] Voice/OCR adoption >40%
- [ ] Revenue potential validated
- [ ] User requests for group buying
- [ ] Technical infrastructure stable

**If NO:** Double down on individual features
**If YES:** Proceed to Phase 3

### After Phase 3 (Week 36)

**Decision:** Scale or focus?

**Criteria:**
- [ ] 10,000+ active users
- [ ] Revenue >$10K/month
- [ ] Group buying traction
- [ ] Partnership interest
- [ ] Clear path to profitability

**If NO:** Focus on core users, optimize
**If YES:** Proceed to Phase 4

---

## Timeline Visualization

```
Month 1-2:  [======== MVP Development ========]
Month 3:    [= MVP Beta Testing =][= Launch =]
Month 4-6:  [======== Phase 2 Features ========]
Month 7:    [= Phase 2 Testing =][= Launch =]
Month 8-11: [======== Phase 3 Community ========]
Month 12:   [= Phase 3 Testing =][= Launch =]
Month 13+:  [======== Phase 4 Scale & Monetize ========]
```

---

## Success Definition

**By End of Year 1:**

- 50,000+ active users
- $50K+ monthly revenue
- 5+ vendor partnerships
- 80%+ 7-day retention
- NPS score >40
- Clear path to $1M+ ARR in Year 2

**One list. Every store. Real money control.**
