# Shopping List Aggregator

**One list. Every store. Real money control.**

A shared shopping list app that turns chaotic household needs into a smart, budget-aware, shoppable plan—across any store, any household, or any group.

---

## The Problem

Most "shopping list" apps solve remembering items. But people don't fail because they forget—they fail because:

- **Help exists in fragments** - One list for groceries, another for Target, sticky notes for hardware stores
- **Small issues interrupt momentum** - Budget overruns, duplicate purchases, unclear priorities
- **Asking feels awkward** - "Can you grab milk?" texts clutter group chats
- **Financial tools punish** - No budget awareness until checkout, leading to cart abandonment
- **No shared decision system** - Multiple people, multiple stores, mixed intents (food, kids, home, work)

People don't need another list. **They need a shared decision system for buying.**

---

## The Solution

Shopping List Aggregator sits between intention and checkout:
- **Before money is spent**
- **Before conflict**
- **Before waste**
- **Before duplicate purchases**

### Core Innovation

One shared list with multiple views:
- View by **Store** (Target, Kroger, Home Depot)
- View by **Aisle** (in-store shopping mode)
- View by **Group/Tag** (Kids, Dinner, Remodel)
- View by **Recipe** (ingredients auto-added)
- View by **Priority** (Must → Nice-to-have)
- View by **Budget** (buy within budget mode)

**This is not multiple lists—it's multiple lenses on the same truth.**

---

## Key Features

### 1. Flexible Capture
- **Quick Entry**: "milk, cereal, bananas" auto-splits
- **Voice Capture**: Natural language parsing
- **Recipe Import**: Auto-add ingredients with cost estimates
- **Product Selection**: Optional exact item selection with photos

### 2. Smart Budget Controls
- Real-time estimated total as you add items
- Priority levels: Must / Should / Nice-to-have
- Over-budget intelligence suggests removals
- Recurring items forecast future needs

### 3. Group Buying
- Pool demand across households
- One shopper, multiple households
- Split costs automatically
- Track contributions

### 4. Shopping Modes
- **In-Store**: Full-screen checklist ordered by aisle
- **Digital**: Direct integrations (Instacart, DoorDash, Walmart, Target, Uber)
- **Offline-Friendly**: Works without cell service

### 5. Receipt Scanning
- OCR parses line items
- Updates last price paid
- Shows budget vs actual
- Improves future estimates

---

## Target Users

### Primary
- **Households / Couples / Roommates** - Shared groceries, Target runs, household basics
- **Parents & Caregivers** - Kids items, meals, recurring needs, budget constraints
- **Home Projects / Remodels** - Home Depot / Lowe's runs organized by room or phase

### Secondary (High-leverage)
- **Neighborhood / Community Runs** - One person shops for multiple households
- **Small Teams** - Office snacks, supplies, events

---

## Business Model

**Revenue from vendors, not users.**

- Vendor partnerships and referrals
- Fulfillment margins on digital orders
- Data insights for retailers (aggregated, anonymous)
- Premium features (optional, not required)

**User value proposition:** Free shared lists + budget control + group buying = household operating system.

---

## Why This Wins

| Feature | Competitors | Shopping List Aggregator |
|---------|-------------|--------------------------|
| **Multi-store support** | Single store or manual | Native cross-store views |
| **Budget awareness** | Post-checkout only | Real-time, before spending |
| **Group buying** | Not supported | Core feature with cost splitting |
| **Priority levels** | Binary (on list or not) | Must / Should / Nice-to-have |
| **Receipt scanning** | Manual or missing | Auto-parse with price learning |
| **Community pooling** | Not available | Neighborhood demand aggregation |

---

## Market Opportunity

- **Global group-buying market**: $19.15B (2025) → $37.03B (2034) at 7.6% CAGR
- **Shopping list app market**: $1.32B (2024) → $6.54B (2033) at 18.7% CAGR
- **North America share**: ~25% of group-buying market with 6.9% CAGR
- **Driver**: Cost-conscious consumers, smartphone penetration, social commerce

**Gap in market:** No platform combines budget-aware lists, multi-store aggregation, and community pooling.

---

## Technology Stack

- **Frontend**: React Native (iOS + Android) or Progressive Web App
- **Backend**: Node.js / Python (FastAPI)
- **Database**: PostgreSQL with Redis caching
- **Real-time**: WebSocket connections for live sync
- **Integrations**: Instacart API, store APIs, receipt OCR services
- **Hosting**: AWS / Google Cloud / Vercel

---

## Getting Started

This repository contains comprehensive documentation:

| Document | Description |
|----------|-------------|
| [FEATURES.md](./FEATURES.md) | Detailed feature specifications |
| [ARCHITECTURE.md](./ARCHITECTURE.md) | Technical design and system architecture |
| [ROADMAP.md](./ROADMAP.md) | Phased implementation plan with timelines |
| [MVP_SPEC.md](./MVP_SPEC.md) | Minimum viable product definition |
| [MARKET_ANALYSIS.md](./MARKET_ANALYSIS.md) | Market research and competitive analysis |

---

## Status

**Phase:** Documentation and Planning
**Next Milestone:** MVP Development

---

## Contact

For questions, partnerships, or investment inquiries, please contact the project team.

---

*One list. Every store. Real money control.*
