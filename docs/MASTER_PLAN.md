# One List: Research & Implementation Plan

## Executive Summary

Build a **budget-aware shared shopping platform** that provides:
1. **One shared list across all stores** - Not fragmented lists, but multiple lenses on the same truth
2. **Real-time budget awareness** - Know your total BEFORE checkout, not after
3. **Priority-driven decisions** - Must/Should/Nice-to-have drives tradeoffs
4. **Pattern learning** - Receipts teach the system, improving estimates over time
5. **Group buying** - Pool demand across households, split costs automatically

**Measurable Win**: Budget adherence rate - % of users who stay within budget after using tradeoff features
- Track: budget set → estimated total → over-budget action taken → actual receipt total
- Target: 70%+ of users stay within 10% of budget

**Target**: Budget-conscious households, busy parents, home project shoppers who want to CONTROL their spending.

**Architecture**: React/Next.js frontend + Node.js/Express backend + PostgreSQL + Redis

**Platform**: Web-first (responsive PWA) with full feature set at launch

**MVP Includes Everything**: Voice capture, OCR receipts, recipes, recurring items, delivery integrations, group buying/pooling, approval controls - all in MVP. No deferred features.

---

## Table of Contents

1. [Executive Summary](#executive-summary)
2. [Competitive Landscape Research](#competitive-landscape-research)
3. [User Requirements](#user-requirements)
4. [Architecture Design](#architecture-design)
5. [Core Innovation: The One List System](#core-innovation-the-one-list-system)
6. [Feature System](#feature-system-capture--decide--shop--close-loop--scale-up)
7. [Real-Value Innovations](#real-value-innovations-integrated-throughout)
8. [Feature Milestones](#feature-milestones-full-mvp---no-phasing)
9. [AI/ML Integration](#aiml-integration)
10. [Database Schema](#database-schema)
11. [Critical Files to Create](#critical-files-to-create)
12. [Differentiation Summary](#differentiation-summary)
13. [UI Scaffolding Prompt](#ui-scaffolding-prompt-boltnew--v0)
14. [Implementation Roadmap](#implementation-roadmap-phased)
15. [Complete Application Sitemap](#complete-application-sitemap)
    - [Site Architecture Overview](#site-architecture-overview)
    - [Complete Page Index](#complete-page-index-27-routes)
    - [Navigation Patterns](#navigation-patterns)
    - [Keyboard Shortcuts](#keyboard-shortcuts)
    - [Page Specifications (ASCII Wireframes)](#page-specifications-ascii-wireframes)
16. [Open Questions / Decisions](#open-questions--decisions)
17. [Sources](#sources)

---

## Competitive Landscape Research

### Feature Comparison Matrix

| Platform | Pricing | Strengths | Weaknesses | Budget Integration |
|----------|---------|-----------|------------|-------------------|
| **AnyList** | Free/$12/yr | Multi-store, recipe import, sync | No budget tracking, no group buying | None |
| **OurGroceries** | Free/$5 | Real-time sync, simple UI | No budget, basic features | None |
| **Bring!** | Free | Collaborative lists, European focus | No budget, groceries only | None |
| **Listonic** | Free/$2/mo | Store-specific lists | No cross-store aggregation | None |
| **Cozi** | Free/$30/yr | Family organization + lists | Not list-focused, cluttered | None |
| **Mint/YNAB** | Varies | Budget tracking, financial tools | No shopping list, post-spend only | Post-checkout |
| **Instacart** | Service fees | Delivery, product matching | Single-store focus, no budget | None |

### Key Insight: Our Differentiator

**NOT** "another shopping list app" - AnyList, OurGroceries already do that.
**NOT** "AI that knows your preferences" - Feature creep, not core value.

**Our claim**: The only app that combines:
1. **Budget awareness BEFORE checkout** - Not after-the-fact like Mint/YNAB
2. **Multi-store aggregation** - One list, filtered by store
3. **Priority-driven tradeoffs** - Must/Should/Nice-to-have
4. **Group buying / neighborhood pooling** - Core differentiator, network effects
5. **Price learning from receipts** - Gets smarter over time

### Market Position Analysis

```
               High Budget Awareness
                       │
                       │
        ONE LIST  ★    │  Budgeting Apps
        (our position) │  (Mint, YNAB)
                       │
                       │
───────────────────────┼───────────────────────
                       │
        List Apps      │  Delivery Apps
        (AnyList,      │  (Instacart,
         OurGroceries) │   DoorDash)
                       │
               Low Budget Awareness
```

**Gap in market**: No app occupies the high budget awareness + multi-store support quadrant.

---

## User Requirements

### The Core Problem (What the market still fails at)

Most "shopping list" apps solve remembering items.
Some meal apps solve what to cook.
A few budget apps solve tracking spend after the fact.

**None solve the real-life system people live in:**
- Multiple people
- Multiple stores
- Mixed intents (food, kids, home, work, clothes, furniture, emergencies)
- Budget tension BEFORE checkout
- One person often shopping for others
- Recurring needs + surprises
- Fragmented tools that don't talk to each other

**People don't need another list. They need a shared decision system for buying.**

### Pain Points Identified

| Pain Point | Current Workaround | Impact |
|------------|-------------------|--------|
| **Fragmented lists** | Sticky notes, multiple apps, texts | Duplicates, forgotten items |
| **Budget uncertainty** | Guess until checkout | Overspending, cart abandonment |
| **Coordination chaos** | "Did you get milk?" texts | Duplicates, frustration |
| **Multi-store complexity** | Mental tracking | Inefficient trips |
| **No shared decisions** | Conflict at checkout | Budget stress |

### The Mental Model (How users think)

People think in **needs**, not SKUs:
- "We need dinners this week."
- "The kids need clothes."
- "We're remodeling the bathroom."
- "I'm short on time—just order it."
- "We can't go over $150."

**The app mirrors this reality.**

### Priority Features Requested

1. **Shared list with real-time sync** - Changes appear instantly for all members
2. **Budget tracking before checkout** - Know your total as you add items
3. **Multi-store views** - Same list, filtered by store
4. **Priority levels** - Must/Should/Nice-to-have
5. **Recipe import** - Add ingredients from recipes
6. **Voice capture** - Hands-free input
7. **Receipt scanning** - Close the loop, learn prices
8. **Group buying** - Pool demand across households

---

## Architecture Design

### Technology Stack

| Component | Technology | Rationale |
|-----------|------------|-----------|
| **Framework** | React + TypeScript | Type safety, component reusability, large ecosystem |
| **Build Tool** | Next.js 14+ (App Router) | SSR, file-based routing, optimized builds |
| **UI Library** | Tailwind CSS + shadcn/ui | Rapid development, consistent design system |
| **State Management** | Zustand | Simple, performant, less boilerplate than Redux |
| **Real-Time** | Socket.io | WebSocket support with fallback to long-polling |
| **Forms** | React Hook Form + Zod | Type-safe form validation |
| **Date/Time** | date-fns | Lightweight, tree-shakeable |

### Backend

| Component | Technology | Rationale |
|-----------|------------|-----------|
| **Framework** | Node.js + Express | Fast, JavaScript full-stack, large ecosystem |
| **Real-Time** | Socket.io server | Bidirectional real-time communication |
| **Authentication** | JWT + bcrypt | Stateless auth, secure password hashing |
| **Validation** | Zod (shared with frontend) | Single source of truth for schemas |
| **Task Queue** | Bull (Redis-backed) | Background jobs (receipts, notifications) |

### Database

| Component | Technology | Rationale |
|-----------|------------|-----------|
| **Primary DB** | PostgreSQL | ACID compliance, JSON support, mature ecosystem |
| **Cache Layer** | Redis | Fast in-memory cache for sessions and real-time data |
| **Search** | PostgreSQL Full-Text Search (MVP) | Built-in, no extra service for MVP |

### Infrastructure

| Component | Technology | Rationale |
|-----------|------------|-----------|
| **Hosting** | Vercel (frontend) + Railway (backend) | Simple deployment, auto-scaling, affordable |
| **CDN** | Cloudflare | Global edge caching, DDoS protection |
| **File Storage** | AWS S3 / Cloudflare R2 | Receipt images |
| **Monitoring** | Sentry (errors) + PostHog (analytics) | Error tracking, user behavior insights |

### External Services

| Service | Purpose | Phase |
|---------|---------|-------|
| **OCR** | Mindee / AWS Textract | Receipt scanning | MVP |
| **NLP** | OpenAI API / Claude | Voice capture parsing | MVP |
| **Email** | Resend / SendGrid | Transactional emails | MVP |
| **Payments** | Stripe | Group buying splits, future monetization | MVP |

---

### High-Level Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                         CLIENT LAYER                            │
├─────────────────────────────────────────────────────────────────┤
│  Web App (React + TypeScript + Tailwind)                        │
│  - Desktop browsers                                             │
│  - Mobile browsers (responsive PWA)                             │
│  - Offline support (Service Worker + IndexedDB)                 │
└──────────────────────┬──────────────────────────────────────────┘
                       │ HTTPS + WebSocket
                       ▼
┌─────────────────────────────────────────────────────────────────┐
│                        API GATEWAY LAYER                        │
├─────────────────────────────────────────────────────────────────┤
│  Cloudflare (CDN + DDoS protection)                             │
│  - Rate limiting                                                │
│  - SSL termination                                              │
│  - Caching                                                      │
└──────────────────────┬──────────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────────┐
│                      APPLICATION LAYER                          │
├─────────────────────────────────────────────────────────────────┤
│  Node.js API Server (Express)                                   │
│  ├── REST API (CRUD operations)                                 │
│  ├── WebSocket Server (real-time sync)                          │
│  ├── Authentication (JWT)                                       │
│  └── Background Workers (receipt processing, emails)            │
└──────────────────────┬──────────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────────┐
│                         DATA LAYER                              │
├─────────────────────────────────────────────────────────────────┤
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐ │
│  │  PostgreSQL     │  │  Redis          │  │  S3             │ │
│  │  (Primary DB)   │  │  (Cache/Queue)  │  │  (File Storage) │ │
│  │  - Users        │  │  - Sessions     │  │  - Receipts     │ │
│  │  - Lists        │  │  - Real-time    │  │  - Images       │ │
│  │  - Items        │  │  - Bull queue   │  │                 │ │
│  │  - Receipts     │  │                 │  │                 │ │
│  │  - Price History│  │                 │  │                 │ │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

### Component Interactions

```
User Action: "Add milk to list"
     │
     ├──> Frontend (React)
     │     ├── Optimistic UI update (instant feedback)
     │     └── Send to backend via WebSocket
     │
     ├──> Backend (Node.js)
     │     ├── Validate request
     │     ├── Calculate new budget total
     │     ├── Save to PostgreSQL
     │     ├── Update Redis cache
     │     └── Broadcast to all connected clients
     │
     └──> Other Users' Frontends
           └── Receive update via WebSocket → UI updates
```

---

## Core Innovation: The One List System

### The Mental Model

**This is NOT multiple lists—it's multiple lenses on the same truth.**

One list can be viewed or filtered as:
- **By Store** (Target, Kroger, Home Depot)
- **By Aisle** (in-store shopping mode)
- **By Group/Tag** (Kids, Dinner, Remodel, Emergency, Party)
- **By Recipe** (meal planning)
- **By Person** (who added what)
- **By Priority** (Must → Should → Nice-to-have)
- **By Budget** (buy within budget mode)

### Core Objects (The System)

#### Lists (the anchor)

One list can include anything. Each item can belong to:
- A store (optional, assigned later)
- A group/tag (Kids, Dinner, Remodel, Emergency, Party)
- A recipe/blueprint
- A priority level

#### Items

Each item supports:
| Field | Description |
|-------|-------------|
| **Name** | What to buy |
| **Quantity** | How many/much |
| **Store** | Where to buy (optional, assigned later) |
| **Group/Tag(s)** | Category for filtering |
| **Priority** | Must / Should / Nice-to-have |
| **Added By** | Who added this item |
| **Notes** | Additional context |
| **Price Estimate** | Manual or learned |
| **Last Price Paid** | From receipt scanning |
| **Status** | Pending / In Cart / Purchased |

#### The Budget Spine

Budget is not a feature—it's the spine that runs through everything:
- Always visible in the UI
- Updates in real-time as items are added
- Drives tradeoff decisions
- Learns from receipts

---

## Feature System: Capture → Decide → Shop → Close Loop → Scale Up

### CAPTURE

#### 1. Quick Entry (Fastest)

**Input:**
```
milk, cereal, bananas
```

**Output:**
- 3 separate items created
- Auto-assigned to default store (if set)
- Default priority: "Should"

| Feature | Behavior |
|---------|----------|
| **Comma separation** | Each comma creates new item |
| **Quantity detection** | "2 milk" → quantity: 2 |
| **Store hints** | "target: paper towels" → assigns to Target |
| **Priority hints** | "!eggs" → priority: Must |

#### 2. Voice Capture (Natural Language)

**User speaks:**
> "Add chicken thighs, rice, and broccoli for dinner. Remove cookies. Add diapers size 4."

**System:**
- Parses intent (add, remove, modify)
- Creates/removes items
- Applies tags ("Dinner")
- Shows review screen before committing

| Capability | Example |
|------------|---------|
| **Add items** | "Add milk and eggs" |
| **Remove items** | "Remove cookies" |
| **Change quantities** | "Change milk to 2 gallons" |
| **Set priorities** | "Mark eggs as must-have" |
| **Assign stores** | "Add paint from Home Depot" |
| **Apply tags** | "Add chicken for dinner" |

#### 3. Recipe / Meal Import

**User selects:** Recipe: "Spaghetti Carbonara"

**System imports:**
```
Ingredients:
- Spaghetti (1 lb) → $2.50
- Bacon (8 oz) → $5.00
- Eggs (4 ea) → $1.20
- Parmesan (4 oz) → $4.00
─────────────────────
Estimated Total: $12.70
Cost per Serving (4): $3.18
```

| Feature | Description |
|---------|-------------|
| **Ingredient Scaling** | Adjust servings (2x, 3x, etc.) |
| **Substitutions** | Swap ingredients to hit budget |
| **Check Pantry** | Mark items you already have |
| **Add to List** | One-click add all ingredients |

### DECIDE

#### Budget Controls

| Level | Description | Use Case |
|-------|-------------|----------|
| **Per List** | One budget for entire list | "Weekly groceries: $150" |
| **Per Store** | Budget per store | "Kroger: $100, Target: $50" |
| **Per Group** | Budget per tag | "Kids: $60, Home: $40" |
| **Per Person** | Individual spending limits | "John: $75, Sarah: $75" |

#### Live Budget Awareness

**Estimated Total Updates:**
```
Items Added:
─────────────────────
Milk              $4.50
Eggs              $3.00
Bread             $2.50
Coffee            $8.00
Butter            $4.00
Cheese            $6.50
─────────────────────
Subtotal:        $28.50
Est. Total:      $28.50 / $150.00
Remaining:      $121.50
```

**Price Sources (in order):**
1. Last price paid (from receipts)
2. Manual estimates
3. Category defaults (e.g., "milk ~$4.50")

#### Priority-Driven Decisions

Each item can be:
- **Must** - Essential, won't remove
- **Should** - Important but flexible
- **Nice-to-have** - Remove if over budget

#### Over-Budget Intelligence

When estimated total > budget:

```
⚠️ Over Budget by $28

Suggestions:
1. Remove 6 Nice-to-have items (-$28)
   → Cookies, Ice Cream, Chips, Soda, Candy, Gum

2. Switch to store brands (-$12)
   → Name brand milk → Store brand
   → Name brand cereal → Store brand
   Still over by $16

3. You skipped Cookies last 3 times
   → Consider removing ($5.00)

[Apply Suggestion 1] [Customize] [Ignore]
```

#### Duplicate Detection (Innovation)

When adding an item that was recently purchased:
```
⚠️ Heads up: You bought eggs 3 days ago.
   Still want to add?
   [Yes, Add] [Skip]
```

### SHOP

#### In-Store Shopping Mode

**Full-Screen Checklist:**
```
┌─────────────────────────────────────┐
│  KROGER                    8 items  │
│  Est: $47.50                        │
├─────────────────────────────────────┤
│                                     │
│  AISLE 1: DAIRY                     │
│  ☐  Milk                     $4.50  │
│  ☐  Eggs                     $3.00  │
│  ☐  Butter                   $4.00  │
│  ☐  Cheese                   $6.50  │
│                                     │
│  AISLE 5: BAKERY                    │
│  ☐  Bread                    $2.50  │
│                                     │
│  AISLE 8: BEVERAGES                 │
│  ☐  Coffee                   $8.00  │
│                                     │
│  AISLE 12: SNACKS                   │
│  ☐  Cookies                  $5.00  │
│  ☐  Ice Cream                $6.00  │
│                                     │
│  [Must Items First ▼]  [Done]       │
└─────────────────────────────────────┘
```

**Design for real-world use:**
- Large tap targets (thumb-friendly)
- Offline-friendly (syncs when connected)
- "Must items first" toggle
- One-hand operation
- Works in fluorescent lighting
- No thinking required

#### Digital Shopping (Full Integration at Launch)

**Export List**
- "Open in Instacart" deep link
- "Open in DoorDash" deep link
- Clean export format
- List remains source of truth

**Product Matching**
- Pre-build cart with matched products
- Show alternatives if item unavailable
- User reviews before checkout
- Automatic product suggestions based on name matching

**Checkout Integration**
- Partnership with Instacart, DoorDash, Walmart, Target
- Checkout within app where supported
- Track delivery status
- Auto-update list when order placed

### CLOSE THE LOOP

#### Receipt Scanning

**After shopping:**
1. User uploads receipt photo
2. OCR extracts: items, prices, total, store, date
3. Auto-matches items to list
4. Marks purchased items complete
5. Updates last price paid

**Output:**
```
RECEIPT ANALYSIS
─────────────────────
Matched Items:
  ✓ Milk          Est: $4.50  Act: $4.79
  ✓ Eggs          Est: $3.00  Act: $2.89
  ✓ Bread         Est: $2.50  Act: $2.99
  ✓ Coffee        Est: $8.00  Act: $7.49

Unmatched Items (not on list):
  ? Bananas                   $1.50
  ? Yogurt                    $3.99

Budget vs Actual:
  Estimated:     $28.50
  Actual:        $30.94
  Difference:    +$2.44 (over by 8.6%)

[Confirm] [Edit Matches] [Add Missing Items]
```

#### Price Learning (Innovation)

System automatically:
- Detects frequently bought items
- Improves future estimates
- Learns store-specific pricing
- Notes price changes: "Milk was $4.50 last time, now $4.79"

### SCALE UP

#### Recurring Items

| Recurrence | Example |
|------------|---------|
| **Weekly** | Milk every Sunday |
| **Bi-weekly** | Eggs every other Thursday |
| **Monthly** | Diapers on the 1st |
| **Custom** | Cleaning supplies every 6 weeks |

**System Behavior:**
- Auto-adds to lists
- Accounts for them in budget forecasts
- Can be paused or edited anytime
- Learns patterns from purchase history

#### Group Buying / Neighborhood Pooling

**Use Case:** One shopper, multiple households

**Setup:**
```
Group: "Oak Street Neighbors"
├── Household 1 (Johnsons)
│   Budget: $50
│   Items: Milk, eggs, bread, coffee
├── Household 2 (Smiths)
│   Budget: $75
│   Items: Diapers, wipes, formula
└── Household 3 (Browns)
    Budget: $60
    Items: Produce, meat, snacks

Shopper: Sarah (Johnson household)
Total Pool: $185
```

**Workflow:**
```
1. Create group "Oak Street Neighbors"
2. Invite households via link
3. Each household adds their items + budget
4. System aggregates into one master list
5. Shopper (Sarah) sees combined list
6. Sarah shops, uploads receipt
7. System calculates splits:
   - Johnsons owe: $48.50
   - Smiths owe: $73.25
   - Browns owe: $61.00
8. Venmo/PayPal links sent automatically
```

**Cost Splitting:**
```
Receipt Total: $182.75

Itemized per Household:
  Johnsons:
    Milk ($4.79), Eggs ($2.89), Bread ($2.99), Coffee ($7.49)
    + Shared items (tax, bags): $1.34
    Total: $20.50

  Smiths:
    Diapers ($35.00), Wipes ($6.00), Formula ($28.00)
    + Shared items: $4.25
    Total: $73.25

  Browns:
    Produce bundle ($18.00), Chicken ($12.00), Snacks ($15.00)
    + Shared items: $3.00
    Total: $48.00

Shopper (Sarah) paid: $182.75
Reimbursement needed:
  - Smiths owe Sarah: $73.25
  - Browns owe Sarah: $48.00
```

#### Approval Rules

| Rule | Description |
|------|-------------|
| **Price threshold** | Items over $50 require approval |
| **Per-person caps** | Each member has a spending limit |
| **Category limits** | "Nice-to-have" capped at $30 |
| **Store caps** | Per-store budget limits with alerts |
| **Group approval** | Pooling purchases require majority approval |

---

## Real-Value Innovations (Integrated Throughout)

### What We're NOT Building (Gimmicky/Over-engineered)

| Feature | Why Skip |
|---------|----------|
| Gamification badges/streaks | Not about points, about control |
| Social feeds / influencer content | Not Instagram for groceries |
| Complex AI chatbots | Over-engineered, just need simple parsing |
| Achievement systems | Patronizing |
| Carbon footprint tracking | Niche, not core value |
| Product catalog browsing | Items come from user input |

### What We ARE Building (Real Value - All MVP)

| Innovation | Value |
|------------|-------|
| **Price Memory** | Better estimates from receipts |
| **Over-Budget Suggestions** | Smart tradeoff recommendations based on priority + past behavior |
| **Duplicate Detection** | "You already bought eggs 3 days ago" |
| **Pattern Learning** | Pre-suggest recurring items based on purchase patterns |
| **Store Brand Alternatives** | Practical money-saving when over budget |
| **Budget Forecasting** | Monthly spend prediction based on recurring items |
| **Voice Capture with NLP** | Hands-free natural language item entry |
| **Receipt OCR** | Automatic receipt scanning and price learning |
| **Delivery Integration** | One-click order through Instacart, DoorDash |
| **Neighborhood Pooling** | Group buying with automatic cost splitting |

**Principle:** Every innovation must answer: "Does this help users control their spending?" If no, don't build it.

---

## Feature Milestones (Full MVP - No Phasing)

**Everything ships in MVP. No deferred features. Full automation from day one.**

### Milestone 1 - Foundation Layer

**Goal:** Core infrastructure and routing

- Routes: implement full route map from SITEMAP
- Global layout: header, nav, list context, budget spine
- All pages render (no stubs, full implementations)
- States: empty, error, offline, and sync conflict states
- Testing: route smoke tests + layout snapshot
- Auth: signup, login, onboarding step rail + invite flow

**Exit Criteria:**
- [ ] Every route renders with full UI
- [ ] Auth flow complete
- [ ] Budget spine visible on all list pages

### Milestone 2 - Core Shopping System

**Goal:** Complete list management with real-time sync

- Items: quick add with comma-split, quantity detection, store hints, priority hints
- Budget: per-list, per-store, per-group, per-person budgets
- Budget spine: real-time updates, over-budget banner, suggestions
- Store view: assignment, filter, unassigned bucket, bulk actions
- Shop mode: full-screen checklist, aisle ordering, offline-friendly, large tap targets
- Real-time sync: Socket.io for instant multi-device updates
- Groups: create, members, invite link, roles, one active list + archives

**Exit Criteria:**
- [ ] End-to-end flow works: create list → add items → budget → shop
- [ ] Real-time sync between devices (< 500ms)
- [ ] Offline mode functional
- [ ] Over-budget intelligence suggests tradeoffs

### Milestone 3 - Voice & NLP Capture

**Goal:** Hands-free natural language input

- Voice capture: microphone input with transcription
- NLP parsing: OpenAI/Claude API for intent extraction
- Actions: add, remove, modify items via voice
- Tag detection: "add chicken for dinner" → applies Dinner tag
- Store detection: "add paint from Home Depot" → assigns store
- Review screen: show parsed actions before committing
- Fallback: paste text input if voice unavailable

**Exit Criteria:**
- [ ] Voice input works on mobile and desktop
- [ ] NLP correctly parses 90%+ of common phrases
- [ ] Review screen shows before committing changes

### Milestone 4 - Receipt OCR & Price Learning

**Goal:** Close the loop with automatic receipt processing

- Receipt upload: drag-drop, camera capture
- OCR: Mindee/Textract extracts items, prices, store, date
- Auto-match: matches receipt items to list items
- Manual match: UI for unmatched items
- Price history: updates "last price paid" automatically
- Budget vs Actual: shows comparison after shopping
- Price learning: improves future estimates based on history
- Duplicate detection: warns if item recently purchased

**Exit Criteria:**
- [ ] OCR accuracy > 90%
- [ ] Auto-matching works for common items
- [ ] Price history updates correctly
- [ ] Budget vs Actual displays correctly

### Milestone 5 - Recipes & Recurring

**Goal:** Meal planning and automated replenishment

- Recipe cards: create, edit, delete recipes
- Recipe URL import: paste URL, auto-extract ingredients
- Ingredient scaling: adjust servings (2x, 3x)
- Add to list: one-click add all ingredients with prices
- Cost per serving: calculate based on estimates
- Recurring rules: weekly, bi-weekly, monthly, custom intervals
- Auto-add: recurring items automatically added to list
- Budget forecasting: predict monthly spend based on recurring items
- Pattern detection: suggest recurring items from purchase history

**Exit Criteria:**
- [ ] Recipe import works for major recipe sites
- [ ] Recurring items auto-add on schedule
- [ ] Budget forecasting shows monthly projection

### Milestone 6 - Delivery Integrations

**Goal:** Order from list through delivery partners

- Export list: clean format for copy/paste
- Deep links: "Open in Instacart", "Open in DoorDash", "Open in Walmart"
- Product matching: suggest products based on item names
- Pre-build cart: create cart with matched products
- Checkout: in-app checkout where supported
- Order tracking: show delivery status
- Auto-update: mark items purchased when order placed

**Exit Criteria:**
- [ ] Export works to all major services
- [ ] Product matching works for common items
- [ ] Order status displays correctly

### Milestone 7 - Group Buying & Pooling

**Goal:** Neighborhood co-op shopping

- Pool groups: create group for multiple households
- Per-household budgets: each household has their own limit
- Combined list: shopper sees aggregated items
- Cost splitting: automatic calculation based on purchases
- Payment integration: Venmo/PayPal links for reimbursement
- Approval rules: price thresholds, per-person caps, category limits
- Shopper assignment: designate who's shopping for the group
- Split payments: automatic reminders and tracking

**Exit Criteria:**
- [ ] Multiple households can join a pool
- [ ] Cost splitting calculates correctly
- [ ] Approval rules enforce limits
- [ ] Payment links work

### Milestone 8 - Polish & Marketing

**Goal:** Public launch readiness

- Marketing pages: home, how-it-works, features, pricing, privacy, terms
- Dashboard: active list snapshot, quick actions, recent activity
- Settings: profile, notifications, preferences
- Help: FAQ, support contact
- Performance: optimize load times, bundle size
- Accessibility: WCAG 2.1 AA compliance
- Mobile optimization: PWA, responsive, touch-friendly

**Exit Criteria:**
- [ ] All marketing pages complete
- [ ] Lighthouse score > 90
- [ ] Mobile experience polished
- [ ] Help content complete

---

## AI/ML Integration

### Voice Capture Agent

**Input:**
```
"Add chicken thighs, rice, and broccoli for dinner. Remove cookies."
```

**Output:**
```json
{
  "actions": [
    { "type": "add", "name": "chicken thighs", "tag": "Dinner" },
    { "type": "add", "name": "rice", "tag": "Dinner" },
    { "type": "add", "name": "broccoli", "tag": "Dinner" },
    { "type": "remove", "name": "cookies" }
  ]
}
```

**Provider:** OpenAI API or Claude

**Implementation:**
```typescript
interface VoiceParseResult {
  actions: Array<{
    type: 'add' | 'remove' | 'modify';
    name: string;
    quantity?: number;
    store?: string;
    tag?: string;
    priority?: 'must' | 'should' | 'nice-to-have';
  }>;
  confidence: number;
}

async function parseVoiceInput(transcript: string): Promise<VoiceParseResult> {
  // Call to AI API with structured output
}
```

### Receipt OCR

**Input:** Receipt photo (JPEG/PNG)

**Output:**
```json
{
  "items": [
    { "name": "Milk", "price": 4.79, "quantity": 1 },
    { "name": "Eggs", "price": 2.89, "quantity": 1 },
    { "name": "Bread", "price": 2.99, "quantity": 1 }
  ],
  "total": 10.67,
  "store": "Kroger",
  "date": "2026-01-09",
  "confidence": 0.94
}
```

**Provider:** Mindee (primary), AWS Textract (fallback)

### Over-Budget Intelligence

**Triggers:** When estimated_total > budget

**Algorithm:**
1. Score each item: priority weight × frequency_skipped × price
2. Rank by score (highest first = most likely to remove)
3. Generate suggestions until under budget
4. Include store brand alternatives if available

**Not ML:** Rule-based with learned weights from user behavior

---

## Database Schema

### Users Table

```sql
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email VARCHAR(255) UNIQUE NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  name VARCHAR(255),
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_users_email ON users(email);
```

### Groups Table (Households)

```sql
CREATE TABLE groups (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name VARCHAR(255) NOT NULL,
  created_by UUID REFERENCES users(id) ON DELETE CASCADE,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE group_members (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  group_id UUID REFERENCES groups(id) ON DELETE CASCADE,
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  role VARCHAR(20) CHECK (role IN ('owner', 'editor', 'viewer')),
  joined_at TIMESTAMP DEFAULT NOW(),
  UNIQUE(group_id, user_id)
);

CREATE INDEX idx_group_members_group_id ON group_members(group_id);
CREATE INDEX idx_group_members_user_id ON group_members(user_id);
```

### Lists Table

```sql
CREATE TABLE lists (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  group_id UUID REFERENCES groups(id) ON DELETE CASCADE,
  name VARCHAR(255) NOT NULL,
  budget DECIMAL(10, 2),
  status VARCHAR(20) CHECK (status IN ('active', 'archived')) DEFAULT 'active',
  created_by UUID REFERENCES users(id) ON DELETE CASCADE,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_lists_group_id ON lists(group_id);
CREATE INDEX idx_lists_status ON lists(status);
```

### Stores Table

```sql
CREATE TABLE stores (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name VARCHAR(255) NOT NULL,
  type VARCHAR(50), -- grocery, home_improvement, pharmacy, etc.
  is_default BOOLEAN DEFAULT FALSE,
  user_id UUID REFERENCES users(id), -- NULL for system stores
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_stores_user_id ON stores(user_id);
```

### Items Table

```sql
CREATE TABLE items (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  list_id UUID REFERENCES lists(id) ON DELETE CASCADE,
  name VARCHAR(255) NOT NULL,
  quantity DECIMAL(10, 2) DEFAULT 1,
  unit VARCHAR(50), -- ea, lb, oz, gal, etc.
  store_id UUID REFERENCES stores(id),
  priority VARCHAR(20) CHECK (priority IN ('must', 'should', 'nice-to-have')) DEFAULT 'should',
  price_estimate DECIMAL(10, 2),
  tags TEXT[], -- PostgreSQL array type
  notes TEXT,
  status VARCHAR(20) CHECK (status IN ('pending', 'in_cart', 'purchased')) DEFAULT 'pending',
  added_by UUID REFERENCES users(id),
  recipe_id UUID, -- If added from a recipe
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_items_list_id ON items(list_id);
CREATE INDEX idx_items_status ON items(status);
CREATE INDEX idx_items_store_id ON items(store_id);
CREATE INDEX idx_items_priority ON items(priority);
```

### Receipts Table

```sql
CREATE TABLE receipts (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  list_id UUID REFERENCES lists(id) ON DELETE CASCADE,
  store_id UUID REFERENCES stores(id),
  image_url VARCHAR(500),
  total_amount DECIMAL(10, 2),
  tax_amount DECIMAL(10, 2),
  receipt_date DATE,
  uploaded_by UUID REFERENCES users(id),
  ocr_raw JSONB, -- Raw OCR output
  processed BOOLEAN DEFAULT FALSE,
  uploaded_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_receipts_list_id ON receipts(list_id);
CREATE INDEX idx_receipts_store_id ON receipts(store_id);
```

### Receipt Items Table

```sql
CREATE TABLE receipt_items (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  receipt_id UUID REFERENCES receipts(id) ON DELETE CASCADE,
  item_id UUID REFERENCES items(id), -- Matched item, can be NULL
  name VARCHAR(255) NOT NULL, -- As it appears on receipt
  price DECIMAL(10, 2) NOT NULL,
  quantity DECIMAL(10, 2) DEFAULT 1,
  matched BOOLEAN DEFAULT FALSE,
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_receipt_items_receipt_id ON receipt_items(receipt_id);
CREATE INDEX idx_receipt_items_item_id ON receipt_items(item_id);
```

### Price History Table

```sql
CREATE TABLE price_history (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  item_name VARCHAR(255) NOT NULL, -- Normalized name
  store_id UUID REFERENCES stores(id),
  price DECIMAL(10, 2) NOT NULL,
  date DATE NOT NULL,
  user_id UUID REFERENCES users(id),
  receipt_id UUID REFERENCES receipts(id),
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_price_history_item_store ON price_history(item_name, store_id);
CREATE INDEX idx_price_history_date ON price_history(date);
```

### Recipes Table

```sql
CREATE TABLE recipes (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  name VARCHAR(255) NOT NULL,
  source_url VARCHAR(500),
  servings INTEGER DEFAULT 4,
  instructions TEXT,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE recipe_ingredients (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  recipe_id UUID REFERENCES recipes(id) ON DELETE CASCADE,
  name VARCHAR(255) NOT NULL,
  quantity DECIMAL(10, 2),
  unit VARCHAR(50),
  price_estimate DECIMAL(10, 2),
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_recipe_ingredients_recipe_id ON recipe_ingredients(recipe_id);
```

### Recurring Rules Table

```sql
CREATE TABLE recurring_rules (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  item_name VARCHAR(255) NOT NULL,
  quantity DECIMAL(10, 2) DEFAULT 1,
  store_id UUID REFERENCES stores(id),
  frequency VARCHAR(20) CHECK (frequency IN ('daily', 'weekly', 'biweekly', 'monthly', 'custom')),
  interval_days INTEGER, -- For custom frequency
  day_of_week INTEGER, -- 0-6 for weekly
  day_of_month INTEGER, -- 1-31 for monthly
  next_add_date DATE,
  is_active BOOLEAN DEFAULT TRUE,
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_recurring_rules_user_id ON recurring_rules(user_id);
CREATE INDEX idx_recurring_rules_next_add_date ON recurring_rules(next_add_date);
```

### Activity Log Table

```sql
CREATE TABLE activity_log (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  list_id UUID REFERENCES lists(id) ON DELETE CASCADE,
  user_id UUID REFERENCES users(id),
  action VARCHAR(50) NOT NULL, -- item_added, item_removed, budget_set, etc.
  entity_type VARCHAR(50), -- item, list, receipt
  entity_id UUID,
  details JSONB, -- Action-specific details
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_activity_log_list_id ON activity_log(list_id);
CREATE INDEX idx_activity_log_created_at ON activity_log(created_at);
```

---

## Critical Files to Create

### Project Structure

```
/web/                          # Next.js Application
├── app/
│   ├── (marketing)/          # Public pages
│   │   ├── page.tsx          # Home
│   │   ├── how-it-works/
│   │   │   └── page.tsx
│   │   ├── features/
│   │   │   └── page.tsx
│   │   ├── pricing/
│   │   │   └── page.tsx
│   │   ├── privacy/
│   │   │   └── page.tsx
│   │   └── terms/
│   │       └── page.tsx
│   ├── (auth)/               # Auth routes
│   │   ├── login/
│   │   │   └── page.tsx
│   │   ├── signup/
│   │   │   └── page.tsx
│   │   └── onboarding/
│   │       └── page.tsx
│   ├── (app)/                # Main app (authenticated)
│   │   ├── layout.tsx        # App shell with nav
│   │   ├── home/
│   │   │   └── page.tsx      # Dashboard
│   │   ├── groups/
│   │   │   ├── page.tsx      # Groups list
│   │   │   └── [groupId]/
│   │   │       └── page.tsx  # Group detail
│   │   ├── lists/
│   │   │   └── [listId]/
│   │   │       ├── page.tsx      # Redirect to /items
│   │   │       ├── items/
│   │   │       │   └── page.tsx  # Main list view
│   │   │       ├── stores/
│   │   │       │   └── page.tsx  # Store view
│   │   │       ├── budget/
│   │   │       │   └── page.tsx  # Budget panel
│   │   │       ├── shop/
│   │   │       │   └── page.tsx  # Shopping mode
│   │   │       ├── receipts/
│   │   │       │   └── page.tsx  # Receipts
│   │   │       ├── history/
│   │   │       │   └── page.tsx  # Activity history
│   │   │       ├── voice/
│   │   │       │   └── page.tsx  # Voice capture
│   │   │       ├── recipes/
│   │   │       │   └── page.tsx  # Recipes
│   │   │       ├── recurring/
│   │   │       │   └── page.tsx  # Recurring items
│   │   │       ├── delivery/
│   │   │       │   └── page.tsx  # Delivery export
│   │   │       ├── pooling/
│   │   │       │   └── page.tsx  # Group buying
│   │   │       └── controls/
│   │   │           └── page.tsx  # Approvals/caps
│   │   └── settings/
│   │       └── page.tsx
│   └── layout.tsx            # Root layout
├── components/
│   ├── list/
│   │   ├── quick-add-input.tsx
│   │   ├── item-row.tsx
│   │   ├── item-list.tsx
│   │   ├── item-detail-sheet.tsx
│   │   ├── priority-badge.tsx
│   │   └── store-chip.tsx
│   ├── budget/
│   │   ├── budget-spine.tsx
│   │   ├── budget-editor.tsx
│   │   ├── over-budget-banner.tsx
│   │   └── budget-progress.tsx
│   ├── shop-mode/
│   │   ├── checklist.tsx
│   │   ├── checklist-item.tsx
│   │   ├── store-selector.tsx
│   │   └── offline-indicator.tsx
│   ├── receipt/
│   │   ├── receipt-upload.tsx
│   │   ├── manual-match-table.tsx
│   │   └── budget-actual-summary.tsx
│   ├── layout/
│   │   ├── app-header.tsx
│   │   ├── app-nav.tsx
│   │   ├── list-tabs.tsx
│   │   └── mobile-nav.tsx
│   └── ui/                   # shadcn/ui components
│       ├── button.tsx
│       ├── input.tsx
│       ├── dialog.tsx
│       ├── dropdown.tsx
│       ├── checkbox.tsx
│       ├── badge.tsx
│       └── ...
├── lib/
│   ├── api-client.ts         # API fetch wrapper
│   ├── socket.ts             # Socket.io client
│   ├── utils.ts              # Utility functions
│   └── hooks/
│       ├── use-list.ts
│       ├── use-items.ts
│       ├── use-budget.ts
│       └── use-realtime.ts
├── stores/                   # Zustand stores
│   ├── list-store.ts
│   ├── user-store.ts
│   └── ui-store.ts
└── package.json

/api/                          # Node.js Backend
├── src/
│   ├── index.ts              # Entry point
│   ├── app.ts                # Express app setup
│   ├── routes/
│   │   ├── auth.ts
│   │   ├── lists.ts
│   │   ├── items.ts
│   │   ├── stores.ts
│   │   ├── receipts.ts
│   │   ├── groups.ts
│   │   ├── recipes.ts
│   │   └── recurring.ts
│   ├── services/
│   │   ├── auth.ts
│   │   ├── budget.ts
│   │   ├── price-learning.ts
│   │   ├── ocr.ts
│   │   └── voice-parse.ts
│   ├── middleware/
│   │   ├── auth.ts
│   │   ├── validate.ts
│   │   └── error-handler.ts
│   ├── db/
│   │   ├── schema.sql
│   │   ├── client.ts
│   │   └── migrations/
│   ├── socket/
│   │   └── realtime.ts
│   └── types/
│       └── index.ts
├── package.json
└── tsconfig.json
```

---

## Differentiation Summary

### What Makes One List Unique

| Differentiator | Description | Competitors |
|----------------|-------------|-------------|
| **Budget-first design** | Budget spine visible everywhere, not an afterthought | None have this |
| **One list, multiple views** | Store/aisle/tag/recipe/priority filters on same data | Listonic has stores, but not unified |
| **Priority-driven tradeoffs** | Must/Should/Nice-to-have enables smart over-budget resolution | None |
| **Price memory** | Receipts teach the system, estimates improve over time | Flipp has prices, not learning |
| **Group buying** | Neighborhood pooling with auto-splits | None |
| **Pattern learning** | Gets smarter over time without being creepy | None |

### What We DON'T Claim (Competitors Already Do)

| Feature | Who Does It |
|---------|-------------|
| "Shared lists" | AnyList, OurGroceries, Bring! |
| "Recipe import" | AnyList, Mealime |
| "Voice input" | Most apps with basic voice |

### Technical Moat

1. **Price learning database** - More receipts = better estimates
2. **Group buying network effects** - More households = more value
3. **Budget intelligence** - Rules + patterns learned from behavior

---

## UI Scaffolding Prompt (Bolt.new / v0)

Use this prompt to generate initial UI scaffolding:

```
Create a modern shared shopping list web application called "One List" using Next.js 14 with App Router, TailwindCSS, and shadcn/ui components.

## Design Style
- Clean, minimal, functional (not flashy)
- Light mode by default with dark mode toggle
- Soft rounded corners (radius-lg)
- Green/teal accent color (money/budget association, e.g., emerald-500)
- Large tap targets for mobile/in-store use
- Clear typography optimized for quick scanning
- System font stack for performance

## Layout Structure (List pages use 2-column on desktop)
- **Left Column (flexible)**: List content, items, checklist
- **Right Column (280px, sticky)**: Budget Spine - always visible budget status
- **Top Navigation**: Logo, search (cmd+K), user avatar
- **Tab Navigation**: Items | Stores | Budget | Shop | Receipts | More
- **Mobile**: Single column, budget spine becomes floating bar at bottom

## Key Pages
1. `/` - Marketing home (hero, how it works, features grid)
2. `/home` - Dashboard with active list snapshot, quick actions, recent activity
3. `/lists/[listId]/items` - Main list view with quick add input, item rows
4. `/lists/[listId]/stores` - Store filter view with chips, unassigned bucket
5. `/lists/[listId]/budget` - Budget panel with editor, price memory
6. `/lists/[listId]/shop` - Full-screen checklist mode (minimal UI)
7. `/lists/[listId]/receipts` - Receipt upload with drag-drop, match table
8. `/groups` - Household/group management

## Components Needed
- Quick add input with comma-split preview (shows items as you type)
- Item row: checkbox, name, qty, price, priority badge, store chip
- Budget spine: Budget | Est | Delta, progress bar
- Over-budget banner with collapse/expand, suggestion buttons
- Shop mode checklist: large checkboxes (48px tap target), offline indicator
- Receipt upload with drag-drop zone, processing spinner
- Tab navigation for list views (horizontal scroll on mobile)
- Empty states with helpful CTAs
- Loading skeletons

## Key Interactions
- Add item: Type in input, press Enter or comma to add
- Check item: Tap checkbox → item moves to "Purchased" section
- Edit item: Tap item → side sheet with details
- Over budget: Banner appears with suggestions
- Shop mode: Full screen, minimal UI, large touch targets

## Tech Stack
- Next.js 14 App Router
- TailwindCSS + shadcn/ui
- Lucide icons
- Zustand for client state
- React Query for server state
- Socket.io for real-time

Start with the main list view (/lists/[listId]/items) with the budget spine and quick add input.
```

---

## Implementation Roadmap (Phased)

Each phase builds on the previous. Every phase includes: implementation, tests, and demo.

---

### Phase 1: Foundation

**Build:** Auth system, database, basic routing

| Create | Test | Demo |
|--------|------|------|
| User signup/login/logout | Auth flow e2e test | User can create account and log in |
| PostgreSQL schema (users, groups, lists, items) | Schema migrations run | Database populated |
| Basic routing structure | All routes render | Navigate between pages |
| Onboarding flow | Onboarding e2e test | New user creates group + first list |

**Exit Criteria:**
- [ ] User can sign up, log in, create group, create list
- [ ] Database persists data correctly
- [ ] All routes accessible (can be stubs)

---

### Phase 2: Core List System

**Build:** The heart of the product - list management with budget spine

| Create | Test | Demo |
|--------|------|------|
| Quick add input with comma-split | Add multiple items via comma | "milk, eggs, bread" adds 3 items |
| Item CRUD (create, read, update, delete) | Item operations e2e | Add, edit, delete items |
| Budget spine component | Budget calculation test | Budget updates as items added |
| Priority levels (Must/Should/Nice) | Priority assignment test | Change item priority |
| Store assignment | Store filter test | Assign items to stores |
| Store view with filter chips | Store grouping test | View items by store |
| Real-time sync (Socket.io) | Multi-device sync test | Two browsers see same changes |

**Exit Criteria:**
- [ ] Can add items via quick add (comma-split works)
- [ ] Budget spine shows running total
- [ ] Can assign stores and filter by store
- [ ] Changes sync between devices in < 500ms

---

### Phase 3: Shopping Mode

**Build:** In-store shopping experience

| Create | Test | Demo |
|--------|------|------|
| Full-screen checklist UI | Checklist render test | Large tap targets, minimal UI |
| Check/uncheck items | Item status toggle test | Check item → moves to purchased |
| Store selector in shop mode | Store filter test | Shop one store at a time |
| Offline support (Service Worker) | Offline mode test | Works without internet |
| Offline sync queue | Sync on reconnect test | Changes sync when back online |

**Exit Criteria:**
- [ ] Shop mode is full-screen with large touch targets
- [ ] Can check off items
- [ ] Works offline
- [ ] Syncs when back online

---

### Phase 4: Groups & Dashboard

**Build:** Multi-user collaboration and home screen

| Create | Test | Demo |
|--------|------|------|
| Group CRUD | Group operations test | Create group, view members |
| Invite link generation | Invite flow test | Share link → user joins group |
| Member roles (owner/editor/viewer) | Permissions test | Viewer can't edit |
| Dashboard with active list snapshot | Dashboard render test | See list summary on home |
| Recent activity feed | Activity log test | "Sam added milk 2m ago" |
| Quick actions | Quick action test | Add items, enter shop mode |
| History/activity log page | History render test | See all list activity |

**Exit Criteria:**
- [ ] Can create groups and invite members
- [ ] Dashboard shows active list summary
- [ ] Recent activity displays correctly
- [ ] Permissions enforced

---

### Phase 5: Voice & Receipts

**Build:** AI-powered capture and close-the-loop

| Create | Test | Demo |
|--------|------|------|
| Voice input UI | Voice UI test | Microphone button, transcription |
| NLP parsing (OpenAI/Claude) | NLP parsing test | "Add milk and eggs for dinner" → 2 items with tag |
| Review screen before commit | Review UI test | Show parsed actions, confirm |
| Receipt upload (drag-drop, camera) | Upload test | Upload receipt image |
| OCR processing (Mindee/Textract) | OCR accuracy test | Extract items + prices |
| Auto-match to list items | Matching algorithm test | Receipt items match list items |
| Manual match UI | Manual match test | Fix unmatched items |
| Price history updates | Price learning test | Last-paid-price updates |
| Budget vs Actual summary | Summary calculation test | Show estimated vs actual |

**Exit Criteria:**
- [ ] Voice input correctly parses natural language
- [ ] Receipt OCR extracts items with 90%+ accuracy
- [ ] Price history updates from receipts
- [ ] Budget vs Actual displays correctly

---

### Phase 6: Recipes & Recurring

**Build:** Meal planning and automation

| Create | Test | Demo |
|--------|------|------|
| Recipe card CRUD | Recipe operations test | Create, edit, delete recipes |
| Recipe URL import | URL parser test | Paste URL → extract ingredients |
| Add ingredients to list | Ingredient add test | One-click add all |
| Serving size scaling | Scaling calculation test | 2x servings → 2x quantities |
| Cost per serving | Cost calculation test | Recipe shows $/serving |
| Recurring rule CRUD | Rule operations test | Create weekly rule |
| Auto-add on schedule | Scheduler test | Items auto-add on date |
| Budget forecasting | Forecast calculation test | Monthly projection |

**Exit Criteria:**
- [ ] Can import recipes from URL
- [ ] Can add recipe ingredients to list
- [ ] Recurring items auto-add on schedule
- [ ] Budget forecasting shows monthly projection

---

### Phase 7: Pooling & Delivery

**Build:** Group buying and delivery integrations

| Create | Test | Demo |
|--------|------|------|
| Export list (copy, Instacart deep link) | Export test | Open list in Instacart |
| DoorDash/Walmart/Target deep links | Deep link test | Export to multiple services |
| Pool group creation | Pool create test | Multiple households join |
| Per-household budgets | Budget aggregation test | Each household has limit |
| Cost splitting calculation | Split calculation test | Calculate per-household totals |
| Payment link generation | Payment link test | Venmo/PayPal links |
| Approval rules | Rule enforcement test | Block items over threshold |
| Per-person spending caps | Cap enforcement test | Alert when cap exceeded |

**Exit Criteria:**
- [ ] Can export list to delivery services
- [ ] Pool groups aggregate multiple households
- [ ] Cost splitting calculates correctly
- [ ] Approval rules enforce limits

---

### Phase 8: Polish & Marketing

**Build:** Public-facing pages and final polish

| Create | Test | Demo |
|--------|------|------|
| Marketing home page | Page render test | Value prop clear |
| How it works page | Page render test | 3-step flow explained |
| Features page | Page render test | Feature grid |
| Pricing page | Page render test | Free tier explained |
| Privacy/Terms pages | Page render test | Legal content |
| Settings page | Settings test | Profile, preferences |
| Help page | Help render test | FAQ, support contact |
| Performance optimization | Lighthouse audit | Score > 90 |
| Accessibility audit | a11y test | WCAG 2.1 AA |
| Mobile polish | Mobile test | PWA works smoothly |

**Exit Criteria:**
- [ ] All marketing pages complete
- [ ] Lighthouse score > 90
- [ ] Accessibility compliant
- [ ] Mobile experience polished

---

### Phase Summary

| Phase | Focus | Key Pages |
|-------|-------|-----------|
| 1 | Foundation | login, signup, onboarding |
| 2 | Core List | items, stores, budget |
| 3 | Shopping | shop |
| 4 | Collaboration | groups, home, history |
| 5 | AI Features | voice, receipts |
| 6 | Automation | recipes, recurring |
| 7 | Scale | delivery, pooling, controls |
| 8 | Launch | marketing, settings, help |

**Total:** 27 routes across 8 phases

---

## Complete Application Sitemap

### Site Architecture Overview

**Route Pattern**: All list-related routes use `/lists/[listId]/...`

```
/                           → Marketing home (redirect to /home if logged in)
├── /how-it-works           → Marketing: How it works
├── /features               → Marketing: Features overview
├── /pricing                → Marketing: Pricing plans
├── /privacy                → Marketing: Privacy policy
├── /terms                  → Marketing: Terms of service
│
├── /login                  → Auth: Sign in
├── /signup                 → Auth: Create account
├── /onboarding
+------------------------------------------------------------------------------+
| HEADER: Back | Onboarding | Step 1 of 4                                      |
+------------------------------------------------------------------------------+
| PROGRESS: [Group]--[List]--[Budget]--[Invite]                                |
+-----------------------------------------+------------------------------------+
| SETUP PANEL                             | LIVE PREVIEW                       |
| GROUP NAME [________________]           | LIST: Weekly Groceries             |
| LIST NAME  [________________]           | Budget: $150 (optional)             |
| BUDGET     [$____] (skip ok)            | Members: You                        |
| INVITE LINK [Copy]  [Add by Email]      | Items: (empty)                      |
| INVITE LIST: [name/email] [Add]         |                                    |
|                                         |                                    |
| ACTIONS                                 |                                    |
| [Back] [Continue] [Skip Invite]         |                                    |
+-----------------------------------------+------------------------------------+
+------------------------------------------------------------------------------+
```

### Complete Page Index (27 Routes)

| # | Route | Purpose | Phase |
|---|-------|---------|-------|
| 1 | `/` | Marketing home | 8 |
| 2 | `/how-it-works` | How it works | 8 |
| 3 | `/features` | Features overview | 8 |
| 4 | `/pricing` | Pricing | 8 |
| 5 | `/privacy` | Privacy policy | 8 |
| 6 | `/terms` | Terms of service | 8 |
| 7 | `/login` | Sign in | 1 |
| 8 | `/signup` | Create account | 1 |
| 9 | `/onboarding` | Initial setup (step rail + live preview) | 1 |
| 10 | `/home` | Dashboard | 4 |
| 11 | `/groups` | Groups list | 4 |
| 12 | `/groups/[groupId]` | Group detail | 4 |
| 13 | `/lists/[listId]/items` | Main list view | 2 |
| 14 | `/lists/[listId]/stores` | Store view | 2 |
| 15 | `/lists/[listId]/budget` | Budget panel | 2 |
| 16 | `/lists/[listId]/shop` | Shopping mode | 3 |
| 17 | `/lists/[listId]/receipts` | Receipts | 5 |
| 18 | `/lists/[listId]/history` | Activity log | 4 |
| 19 | `/lists/[listId]/voice` | Voice capture | 5 |
| 20 | `/lists/[listId]/recipes` | Recipes | 6 |
| 21 | `/lists/[listId]/recurring` | Recurring items | 6 |
| 22 | `/lists/[listId]/delivery` | Delivery export | 7 |
| 23 | `/lists/[listId]/pooling` | Group buying | 7 |
| 24 | `/lists/[listId]/controls` | Approval rules | 7 |
| 25 | `/settings` | User settings | 8 |
| 26 | `/help` | Help/support | 8 |
| 27 | `/lists/[listId]` | Redirect | 2 |

### Navigation Patterns

| From | To | Action |
|------|-----|--------|
| Dashboard | List | Click active list card |
| Any page | Dashboard | Click logo or "Home" |
| List tabs | Other tabs | Click tab name |
| List | Item detail | Click item row → side sheet |
| List | Shop mode | Click "Shop" tab |
| Shop mode | Exit | Click "Exit Shop" |
| Any page | Settings | Avatar → Settings |
| Groups | Group detail | Click group card |

### Keyboard Shortcuts

| Shortcut | Action | Context |
|----------|--------|---------|
| `Cmd+K` | Command Palette | Global |
| `Cmd+N` | Quick Add Focus | List views |
| `Cmd+Enter` | Submit | Forms |
| `Escape` | Close/Cancel | Modals, sheets |
| `Enter` | Add Item | Quick-add input |
| `,` | Split Items | Quick-add (auto-splits on comma) |
| `Space` | Toggle Check | Shop mode checklist |

---

### Page Specifications (ASCII Wireframes)

#### Marketing / Public Pages

```
/
+------------------------------------------------------------------------------+
| LOGO  ONE LIST                                          [Log In] [Sign Up]   |
+------------------------------------------------------------------------------+
|                                                                              |
| HERO: One list. Every store. Real money control.                            |
|                                                                              |
| SUB: Plan together. Spend together. Stay on budget.                         |
|                                                                              |
| [Create Your First List]                                                     |
|                                                                              |
+------------------------------------------------------------------------------+
| HOW IT WORKS                                                                 |
|                                                                              |
| 1. CAPTURE           2. DECIDE            3. SHOP                           |
| Add items any way    Set budget,          Shop with                          |
| you want - quick     prioritize what      full-screen                        |
| add, voice, recipes  matters              checklist                          |
|                                                                              |
+------------------------------------------------------------------------------+
| FEATURES                                                                     |
|                                                                              |
| [Shared Lists] [Budget Tracking] [Multi-Store] [Recipes]                    |
| [Voice Capture] [Receipt Scanning] [Group Buying] [Recurring]               |
|                                                                              |
+------------------------------------------------------------------------------+
| FOOTER: Privacy | Terms | Pricing | GitHub                                   |
+------------------------------------------------------------------------------+
```

### Auth Pages

```
/login
+------------------------------------------------------------------------------+
| LOGO                                                                         |
+------------------------------------------------------------------------------+
|                                                                              |
|                     Welcome back                                             |
|                                                                              |
|                     Email                                                    |
|                     [____________________________]                           |
|                                                                              |
|                     Password                                                 |
|                     [____________________________]                           |
|                                                                              |
|                     [Log In]                                                 |
|                                                                              |
|                     [Forgot Password?]                                       |
|                                                                              |
|                     Don't have an account? [Sign Up]                        |
|                                                                              |
+------------------------------------------------------------------------------+

/signup
+------------------------------------------------------------------------------+
| LOGO                                                                         |
+------------------------------------------------------------------------------+
|                                                                              |
|                     Create your account                                      |
|                                                                              |
|                     Name                                                     |
|                     [____________________________]                           |
|                                                                              |
|                     Email                                                    |
|                     [____________________________]                           |
|                                                                              |
|                     Password                                                 |
|                     [____________________________]                           |
|                                                                              |
|                     [Create Account]                                         |
|                                                                              |
|                     Already have an account? [Log In]                       |
|                                                                              |
+------------------------------------------------------------------------------+

/onboarding
+------------------------------------------------------------------------------+
| LOGO                                                                         |
+------------------------------------------------------------------------------+
|                                                                              |
|                     Let's set up your household                              |
|                                                                              |
|                     Household name                                           |
|                     [The Johnsons_________________]                          |
|                                                                              |
|                     Your first list                                          |
|                     [Weekly Groceries_____________]                          |
|                                                                              |
|                     Budget (optional)                                        |
|                     [$150___]                                                |
|                                                                              |
|                     [Continue]                                               |
|                                                                              |
|                     ─────────────────────────────────────────────            |
|                                                                              |
|                     Invite your household (optional)                         |
|                     [Copy Invite Link]                                       |
|                                                                              |
+------------------------------------------------------------------------------+
```

### App Home / Dashboard

```
/home
+------------------------------------------------------------------------------+
| LOGO  ONE LIST                          Search...      [Notifications] [AV] |
+------------------------------------------------------------------------------+
| NAV: Home | List | Budget | Shop | Receipts | History | Settings             |
+------------------------------------------------------------------------------+
|                                                                              |
| ACTIVE LIST                                                      [View All] |
| +------------------------------------------------------------------------+  |
| | Weekly Groceries                                                        |  |
| |                                                                         |  |
| | Budget: $150.00                                                         |  |
| | Estimated: $128.50                         [================    ] 86%  |  |
| | Remaining: $21.50                                                       |  |
| |                                                                         |  |
| | 12 items | 3 stores | 5 must-have                                      |  |
| |                                                                         |  |
| | [Add Items] [Shop Mode] [View List]                                     |  |
| +------------------------------------------------------------------------+  |
|                                                                              |
| QUICK ACTIONS                                                                |
| +------------+ +------------+ +------------+ +------------+                  |
| | Add Items  | | Shop Mode  | | Budget     | | Invite     |                  |
| +------------+ +------------+ +------------+ +------------+                  |
|                                                                              |
| RECENT ACTIVITY                                                              |
| +------------------------------------------------------------------------+  |
| | - Sam added Milk                                              2m ago    |  |
| | - Alex set budget to $150                                    10m ago    |  |
| | - Priya marked Soap purchased                                 1h ago    |  |
| | - Sam created list "Weekly Groceries"                         1h ago    |  |
| +------------------------------------------------------------------------+  |
|                                                                              |
| ARCHIVED LISTS                                                  [View All]  |
| Last Week Groceries | Home Depot Run                                         |
|                                                                              |
+------------------------------------------------------------------------------+
```

### Main List View (Items)

```
/lists/:listId/items
+------------------------------------------------------------------------------+
| LOGO  ONE LIST                          Search...      [Notifications] [AV] |
+------------------------------------------------------------------------------+
| LIST: Weekly Groceries                               Sync: Online  [Profile] |
| TABS: [Items] | Stores | Budget | Shop | Receipts | History | Voice | More   |
+------------------------------------------------------------------------------+
| QUICK ADD                                                                    |
| [milk, eggs, bread_____________________________________] [Add]               |
+--------------------------------------+---------------------------------------+
| ITEMS (12)                           | BUDGET SPINE                          |
|                                      |                                       |
| MUST (5)                             | Budget: $150.00                       |
| [x] Milk           qty 2     $4.50   | ─────────────────                     |
| [ ] Eggs           qty 1     $3.00   | Est:    $128.50                       |
| [ ] Bread          qty 1     $2.50   | ─────────────────                     |
| [ ] Diapers        qty 1    $35.00   | Delta:  +$21.50                       |
| [ ] Prescription   qty 1    $12.00   |                                       |
|                                      | [================    ] 86%            |
| SHOULD (5)                           |                                       |
| [ ] Coffee         qty 1     $8.00   | ─────────────────                     |
| [ ] Butter         qty 1     $4.00   | Must:      $57.00                     |
| [ ] Cheese         qty 1     $6.50   | Should:    $50.50                     |
| [ ] Chicken        qty 2    $12.00   | Nice:      $21.00                     |
| [ ] Rice           qty 1     $5.00   |                                       |
|                                      | [View Budget →]                       |
| NICE-TO-HAVE (2)                     |                                       |
| [ ] Cookies        qty 1     $5.00   |                                       |
| [ ] Ice Cream      qty 1     $6.00   |                                       |
|                                      |                                       |
+--------------------------------------+---------------------------------------+
| BULK ACTIONS: [Assign Store] [Set Priority] [Add Tag] [Delete Selected]      |
+------------------------------------------------------------------------------+
```

### Over Budget State

```
/lists/:listId/items (over budget)
+------------------------------------------------------------------------------+
| LOGO  ONE LIST                          Search...      [Notifications] [AV] |
+------------------------------------------------------------------------------+
| LIST: Weekly Groceries                               Sync: Online  [Profile] |
| TABS: [Items] | Stores | Budget | Shop | Receipts | History | Voice | More   |
+------------------------------------------------------------------------------+
| ⚠️ OVER BUDGET BY $28.50                                          [Dismiss]  |
| +------------------------------------------------------------------------+  |
| | Suggestions:                                                            |  |
| | 1. Remove Nice-to-have items (-$21)  [Apply]                           |  |
| | 2. You skipped Cookies last 3 times (-$5)  [Remove]                    |  |
| | 3. Switch to store brand milk (-$1.50)  [Apply]                        |  |
| +------------------------------------------------------------------------+  |
+------------------------------------------------------------------------------+
| QUICK ADD                                                                    |
| [_____________________________________________________] [Add]               |
+--------------------------------------+---------------------------------------+
| ITEMS (12)                           | BUDGET SPINE                          |
|                                      |                                       |
| (items list continues...)            | Budget: $150.00                       |
|                                      | ─────────────────                     |
|                                      | Est:    $178.50   ⚠️                  |
|                                      | ─────────────────                     |
|                                      | Over:   -$28.50                       |
|                                      |                                       |
|                                      | [=================>  ] 119%           |
|                                      |                                       |
+--------------------------------------+---------------------------------------+
```

### Store View

```
/lists/:listId/stores
+------------------------------------------------------------------------------+
| LOGO  ONE LIST                          Search...      [Notifications] [AV] |
+------------------------------------------------------------------------------+
| LIST: Weekly Groceries                               Sync: Online  [Profile] |
| TABS: Items | [Stores] | Budget | Shop | Receipts | History | Voice | More   |
+------------------------------------------------------------------------------+
| STORE FILTER                                                                 |
| [All] [Kroger (6)] [Target (3)] [Unassigned (3)] [+ Add Store]              |
+--------------------------------------+---------------------------------------+
| BY STORE                             | BUDGET SPINE                          |
|                                      |                                       |
| KROGER (6 items, $45.00)             | Budget: $150.00                       |
| [ ] Milk           qty 2     $4.50   | ─────────────────                     |
| [ ] Eggs           qty 1     $3.00   | Est:    $128.50                       |
| [ ] Bread          qty 1     $2.50   | ─────────────────                     |
| [ ] Coffee         qty 1     $8.00   | Delta:  +$21.50                       |
| [ ] Butter         qty 1     $4.00   |                                       |
| [ ] Cheese         qty 1     $6.50   | By Store:                             |
|                                      | Kroger   $45.00                       |
| TARGET (3 items, $41.00)             | Target   $41.00                       |
| [ ] Diapers        qty 1    $35.00   | Unassigned $42.50                     |
| [ ] Cookies        qty 1     $5.00   |                                       |
| [ ] Ice Cream      qty 1     $6.00   |                                       |
|                                      |                                       |
| UNASSIGNED (3 items, $42.50)         |                                       |
| [ ] Prescription   qty 1    $12.00   |                                       |
| [ ] Chicken        qty 2    $12.00   |                                       |
| [ ] Rice           qty 1     $5.00   |                                       |
| [Assign All to Store ▼]              |                                       |
|                                      |                                       |
+--------------------------------------+---------------------------------------+
```

### Budget View

```
/lists/:listId/budget
+------------------------------------------------------------------------------+
| LOGO  ONE LIST                          Search...      [Notifications] [AV] |
+------------------------------------------------------------------------------+
| LIST: Weekly Groceries                               Sync: Online  [Profile] |
| TABS: Items | Stores | [Budget] | Shop | Receipts | History | Voice | More   |
+------------------------------------------------------------------------------+
| BUDGET PANEL                                                                 |
| +------------------------------------------------------------------------+  |
| | Budget: [$150.00____] [Update]                                          |  |
| |                                                                         |  |
| | [================    ] 86%                                              |  |
| |                                                                         |  |
| | Estimated Total:    $128.50                                             |  |
| | Remaining:          $21.50                                              |  |
| +------------------------------------------------------------------------+  |
+--------------------------------------+---------------------------------------+
| BREAKDOWN BY PRIORITY                | ACTIONS                               |
|                                      |                                       |
| MUST           $57.00 (38%)          | [Remove Nice-to-have]                 |
| [========            ]               |                                       |
|                                      | [Show Must Only]                      |
| SHOULD         $50.50 (34%)          |                                       |
| [=======             ]               | [Recalculate Estimates]               |
|                                      |                                       |
| NICE-TO-HAVE   $21.00 (14%)          |                                       |
| [===                 ]               |                                       |
|                                      |                                       |
| ─────────────────────────────────────|                                       |
|                                      |                                       |
| PRICE MEMORY                         |                                       |
| Recent prices from receipts:         |                                       |
|                                      |                                       |
| Item        Last Paid   Updated      |                                       |
| ─────────────────────────────────    |                                       |
| Milk        $4.79       Jan 2        |                                       |
| Eggs        $2.89       Jan 2        |                                       |
| Bread       $2.99       Dec 28       |                                       |
| Coffee      $7.49       Dec 28       |                                       |
|                                      |                                       |
+--------------------------------------+---------------------------------------+
```

### Shopping Mode

```
/lists/:listId/shop
+------------------------------------------------------------------------------+
| SHOP MODE: Kroger                                   Offline ○   [Exit Shop] |
+------------------------------------------------------------------------------+
|                                                                              |
| STORE: [All] [Kroger ●] [Target] [Unassigned]              Budget: $45/$150 |
|                                                                              |
+------------------------------------------------------------------------------+
|                                                                              |
|   ┌────────────────────────────────────────────────────────────────────┐    |
|   │                                                                    │    |
|   │   AISLE 1: DAIRY                                                   │    |
|   │                                                                    │    |
|   │   [ ]  Milk                                            $4.50       │    |
|   │                                                                    │    |
|   │   [ ]  Eggs                                            $3.00       │    |
|   │                                                                    │    |
|   │   [ ]  Butter                                          $4.00       │    |
|   │                                                                    │    |
|   │   [ ]  Cheese                                          $6.50       │    |
|   │                                                                    │    |
|   └────────────────────────────────────────────────────────────────────┘    |
|                                                                              |
|   ┌────────────────────────────────────────────────────────────────────┐    |
|   │                                                                    │    |
|   │   AISLE 5: BAKERY                                                  │    |
|   │                                                                    │    |
|   │   [ ]  Bread                                           $2.50       │    |
|   │                                                                    │    |
|   └────────────────────────────────────────────────────────────────────┘    |
|                                                                              |
|   ┌────────────────────────────────────────────────────────────────────┐    |
|   │                                                                    │    |
|   │   AISLE 8: BEVERAGES                                               │    |
|   │                                                                    │    |
|   │   [ ]  Coffee                                          $8.00       │    |
|   │                                                                    │    |
|   └────────────────────────────────────────────────────────────────────┘    |
|                                                                              |
+------------------------------------------------------------------------------+
| PURCHASED (0)                                                       [Done] |
+------------------------------------------------------------------------------+
```

### Receipts View

```
/lists/:listId/receipts
+------------------------------------------------------------------------------+
| LOGO  ONE LIST                          Search...      [Notifications] [AV] |
+------------------------------------------------------------------------------+
| LIST: Weekly Groceries                               Sync: Online  [Profile] |
| TABS: Items | Stores | Budget | Shop | [Receipts] | History | Voice | More   |
+------------------------------------------------------------------------------+
| UPLOAD RECEIPT                                                               |
| +------------------------------------------------------------------------+  |
| |                                                                         |  |
| |        ┌─────────────────────────────────────────────────────┐         |  |
| |        │                                                     │         |  |
| |        │      Drag and drop receipt image here               │         |  |
| |        │                                                     │         |  |
| |        │              or click to browse                     │         |  |
| |        │                                                     │         |  |
| |        │         Supported: JPG, PNG, PDF                    │         |  |
| |        │                                                     │         |  |
| |        └─────────────────────────────────────────────────────┘         |  |
| |                                                                         |  |
| +------------------------------------------------------------------------+  |
+--------------------------------------+---------------------------------------+
| MANUAL MATCH                         | BUDGET VS ACTUAL                      |
|                                      |                                       |
| Receipt Items:                       | Budgeted:    $150.00                  |
|                                      | Estimated:   $128.50                  |
| Item        Price   Match            | Actual:      $135.94                  |
| ───────────────────────────────────  | ─────────────────                     |
| MILK 2%     $4.79   [Milk ▼]  [✓]   | Difference:  -$14.06 (over)           |
| EGGS LG     $2.89   [Eggs ▼]  [✓]   |                                       |
| BREAD WH    $2.99   [Bread ▼] [✓]   | [=================>  ] 91%            |
| COFFEE      $7.49   [Coffee ▼][✓]   |                                       |
| BUTTER      $4.29   [Butter ▼][✓]   |                                       |
| CHEESE      $6.99   [Cheese ▼][✓]   |                                       |
| BANANAS     $1.50   [Not on list]   |                                       |
| YOGURT      $3.99   [Not on list]   |                                       |
|                                      |                                       |
| [Confirm Matches] [Add Missing]      |                                       |
+--------------------------------------+---------------------------------------+
| PAST RECEIPTS                                                                |
| Jan 2 - Kroger - $135.94 | Dec 28 - Target - $45.67                         |
+------------------------------------------------------------------------------+
```

### Groups View

```
/groups
+------------------------------------------------------------------------------+
| LOGO  ONE LIST                          Search...      [Notifications] [AV] |
+------------------------------------------------------------------------------+
| NAV: Home | List | Budget | Shop | Receipts | History | Settings             |
+------------------------------------------------------------------------------+
|                                                                              |
| GROUPS                                                       [Create Group] |
|                                                                              |
| +------------------------------------------------------------------------+  |
| | The Johnsons                                               3 members    |  |
| | Active List: Weekly Groceries                                           |  |
| | Budget: $150 | Est: $128                                                |  |
| +------------------------------------------------------------------------+  |
|                                                                              |
| +------------------------------------------------------------------------+  |
| | Oak Street Neighbors                                       5 members    |  |
| | Active List: Group Buy - January                                        |  |
| | Pool: $350 | Est: $290                                                  |  |
| +------------------------------------------------------------------------+  |
|                                                                              |
+------------------------------------------------------------------------------+

/groups/:groupId
+------------------------------------------------------------------------------+
| LOGO  ONE LIST                          Search...      [Notifications] [AV] |
+------------------------------------------------------------------------------+
| GROUP: The Johnsons                                         [Settings] [⋮]  |
+------------------------------------------------------------------------------+
|                                                                              |
| MEMBERS (3)                                                 [Invite Member] |
| +------------------------------------------------------------------------+  |
| | Sam Johnson                                    Owner                     |  |
| | Alex Johnson                                   Editor                    |  |
| | Grandma Johnson                                Viewer                    |  |
| +------------------------------------------------------------------------+  |
|                                                                              |
| ACTIVE LIST                                                                  |
| +------------------------------------------------------------------------+  |
| | Weekly Groceries                                         [View List →] |  |
| | Budget: $150 | Est: $128 | 12 items                                     |  |
| +------------------------------------------------------------------------+  |
|                                                                              |
| ARCHIVED LISTS                                                               |
| +------------------------------------------------------------------------+  |
| | Last Week Groceries                                Jan 2, 2026          |  |
| | Home Depot Run                                     Dec 28, 2025         |  |
| +------------------------------------------------------------------------+  |
|                                                                              |
+------------------------------------------------------------------------------+
```

### Empty State

```
/lists/:listId/items (empty)
+------------------------------------------------------------------------------+
| LOGO  ONE LIST                          Search...      [Notifications] [AV] |
+------------------------------------------------------------------------------+
| LIST: Weekly Groceries                               Sync: Online  [Profile] |
| TABS: [Items] | Stores | Budget | Shop | Receipts | History | Voice | More   |
+------------------------------------------------------------------------------+
| QUICK ADD                                                                    |
| [milk, eggs, bread_____________________________________] [Add]               |
+--------------------------------------+---------------------------------------+
|                                      | BUDGET SPINE                          |
|                                      |                                       |
|   ┌────────────────────────────┐     | Budget: $150.00                       |
|   │                            │     | ─────────────────                     |
|   │   Your list is empty       │     | Est:    $0.00                         |
|   │                            │     | ─────────────────                     |
|   │   Start by adding items    │     | Remaining: $150.00                    |
|   │   above, or try:           │     |                                       |
|   │                            │     | [                    ] 0%             |
|   │   [Voice] [Recipe]         │     |                                       |
|   │                            │     |                                       |
|   └────────────────────────────┘     |                                       |
|                                      |                                       |
+--------------------------------------+---------------------------------------+
```

### Offline State

```
/lists/:listId/items (offline)
+------------------------------------------------------------------------------+
| ⚠️ You're offline. Changes will sync when you reconnect.            [Dismiss]|
+------------------------------------------------------------------------------+
| LOGO  ONE LIST                          Search...      [Notifications] [AV] |
+------------------------------------------------------------------------------+
| LIST: Weekly Groceries                               Sync: Offline  [Profile]|
| TABS: [Items] | Stores | Budget | Shop | Receipts | History | Voice | More   |
+------------------------------------------------------------------------------+
| QUICK ADD                                                                    |
| [_____________________________________________________] [Add]               |
+--------------------------------------+---------------------------------------+
| ITEMS (12)                           | BUDGET SPINE                          |
|                                      |                                       |
| (items continue normally...)         | Budget: $150.00                       |
|                                      | ─────────────────                     |
|                                      | Est:    $128.50                       |
|                                      |                                       |
+--------------------------------------+---------------------------------------+
| PENDING CHANGES: 3 items will sync when online                               |
+------------------------------------------------------------------------------+
```

---

## Open Questions / Decisions

| Question | Options | Recommendation | Decision |
|----------|---------|----------------|----------|
| **Auth method** | Email/password vs OAuth vs Magic link | Email/password for MVP (simplest) | TBD |
| **Offline strategy** | IndexedDB + service worker | IndexedDB for list data, SW for app shell | TBD |
| **Mobile** | PWA vs Native | PWA for v1, evaluate native for v2 | TBD |
| **Real-time** | WebSocket vs SSE vs polling | WebSocket (Socket.io) | TBD |
| **Aisle ordering** | Manual vs API | Manual for MVP, API integration v2 | TBD |
| **Voice provider** | OpenAI vs Claude vs native | OpenAI Whisper + GPT for parsing | TBD |
| **OCR provider** | Mindee vs Textract vs Google | Mindee (best receipt accuracy) | TBD |

---

## Sources

### Market Research

- Group-buying market size: $19.15B (2025) → $37.03B (2034) at 7.6% CAGR
- Shopping list app market: $1.32B (2024) → $6.54B (2033) at 18.7% CAGR
- Source: December 2025 market report

### Competitor Analysis

- AnyList: https://www.anylist.com
- OurGroceries: https://www.ourgroceries.com
- Bring!: https://www.getbring.com
- Listonic: https://listonic.com
- Cozi: https://www.cozi.com

### Technology Documentation

- Next.js: https://nextjs.org/docs
- Socket.io: https://socket.io/docs
- Tailwind CSS: https://tailwindcss.com/docs
- shadcn/ui: https://ui.shadcn.com
- PostgreSQL: https://www.postgresql.org/docs
- Zustand: https://zustand-demo.pmnd.rs/

### Design Inspiration

- Linear: https://linear.app
- Notion: https://notion.so
- Todoist: https://todoist.com

---

## MVP Rules (Non-Negotiables)

- **Full feature set at launch** - No deferred features, no Phase 2
- Items only come from user input inside a list (no product catalog or store database)
- Store assignment is optional; items are store-agnostic at creation
- One active list per user; prior lists can be archived (read-only)
- Budget stays visible on all list surfaces (the "budget spine")
- **All automation included**: Voice capture, OCR, recipe import, delivery integrations, pooling
- Update docs on every scope change
- Real-time sync is non-negotiable (< 500ms latency)
- Offline mode must work for core shopping flows

---

**One list. Every store. Real money control.**
