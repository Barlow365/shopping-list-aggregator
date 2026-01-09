# Shopping List Aggregator - Features Guide

**Version:** 1.0
**Last Updated:** January 2026

---

## Table of Contents

1. [Core Concepts](#core-concepts)
2. [List Management](#list-management)
3. [Item Capture Methods](#item-capture-methods)
4. [Smart Views](#smart-views)
5. [Budget Controls](#budget-controls)
6. [Shopping Modes](#shopping-modes)
7. [Receipt Scanning](#receipt-scanning)
8. [Group Buying](#group-buying)
9. [Recurring Items](#recurring-items)
10. [Digital Shopping Integration](#digital-shopping-integration)
11. [User Management](#user-management)

---

## Core Concepts

### The Mental Model

People think in **needs**, not SKUs:
- "We need dinners this week."
- "The kids need clothes."
- "We're remodeling the bathroom."
- "I'm short on time—just order it."
- "We can't go over $150."

The app mirrors this reality.

### Core Objects

| Object | Description | Key Fields |
|--------|-------------|------------|
| **List** | The anchor—one list can include anything | Name, budget, members, tags |
| **Item** | Individual thing to buy | Name, quantity, store, price, priority, tags, notes |
| **Store** | Where you shop | Name, type (grocery, home, pharmacy, etc.) |
| **Group/Tag** | Category for items | Kids, Dinner, Remodel, Emergency, Party |
| **Receipt** | Post-purchase record | Items purchased, actual prices, date |

### The System Principle

**This is not multiple lists—it's multiple lenses on the same truth.**

One list can be viewed or filtered as:
- By Store (Target, Kroger, Home Depot)
- By Aisle (shopping mode)
- By Group/Tag (Kids, Dinner, Remodel)
- By Recipe (meal planning)
- By Person (who added what)
- By Priority (Must → Nice-to-have)
- By Budget (buy within budget view)

---

## List Management

### Creating a List

| Feature | Description |
|---------|-------------|
| **Quick Create** | Name + budget → instant list |
| **Templates** | "Weekly Groceries", "Home Project", "Party Supplies" |
| **Sharing** | Invite via link, email, or phone number |
| **Permissions** | Owner, Editor, Viewer roles |

### List Organization

```
List: "Household Needs"
├── Store: Kroger
│   ├── Priority: Must
│   │   ├── Milk ($4.50)
│   │   └── Eggs ($3.00)
│   ├── Priority: Should
│   │   └── Coffee ($8.00)
│   └── Priority: Nice-to-have
│       └── Cookies ($5.00)
├── Store: Target
│   ├── Kids (Tag)
│   │   ├── Diapers ($35.00)
│   │   └── Wipes ($6.00)
│   └── Home (Tag)
│       └── Light bulbs ($12.00)
└── Store: Home Depot
    └── Remodel (Tag)
        ├── Paint ($40.00)
        └── Brushes ($15.00)
```

### List Actions

| Action | Description |
|--------|-------------|
| **Duplicate** | Copy list to start new shopping trip |
| **Archive** | Move completed lists to archive |
| **Share** | Invite collaborators |
| **Export** | Download as CSV or PDF |
| **Print** | Printer-friendly checklist |

---

## Item Capture Methods

### 1. Quick Entry (Fastest)

**Input:**
```
milk, cereal, bananas
```

**Output:**
- 3 separate items
- Auto-assigned to default store (if set)
- Default priority: "Should"

| Feature | Behavior |
|---------|----------|
| **Comma separation** | Each comma creates new item |
| **Quantity detection** | "2 milk" → quantity: 2 |
| **Store hints** | "target: paper towels" → assigns to Target |
| **Priority hints** | "!eggs" → priority: Must |

### 2. Voice Capture (Natural Language)

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

### 3. Recipe / Meal Import

**User selects:**
- Recipe: "Spaghetti Carbonara"

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

### 4. Product-Level Selection (Phase 2)

| Feature | Description |
|---------|-------------|
| **Search Products** | Search "organic milk" → see options |
| **View Photos** | Product images |
| **Compare Prices** | Store brand vs name brand |
| **Select Exact Item** | Lock in specific SKU |

---

## Smart Views

### View by Store

**Filter:** "Show only Kroger items"

```
KROGER (8 items, $47.50)
Must-Have (3)
  ☐ Milk           $4.50
  ☐ Eggs           $3.00
  ☐ Bread          $2.50
Should (3)
  ☐ Coffee         $8.00
  ☐ Butter         $4.00
  ☐ Cheese         $6.50
Nice-to-Have (2)
  ☐ Cookies        $5.00
  ☐ Ice Cream      $6.00
```

### View by Group/Tag

**Filter:** "Show only Kids items"

```
KIDS (12 items, $89.00)
From Target:
  ☐ Diapers size 4    $35.00
  ☐ Wipes             $6.00
  ☐ Baby food (6pk)   $8.00
From Kroger:
  ☐ Milk              $4.50
  ☐ Bananas           $1.50
From Carter's:
  ☐ 2T shirts (3pk)   $15.00
```

### View by Priority

**Filter:** "Must items only"

```
MUST-HAVE ITEMS (15 items, $112.00)
Within Budget: $150
Remaining: $38.00

☐ Milk (Kroger)         $4.50
☐ Eggs (Kroger)         $3.00
☐ Bread (Kroger)        $2.50
☐ Diapers (Target)     $35.00
☐ Prescription (CVS)   $12.00
☐ Gas ($40 worth)      $40.00
...
```

### View by Budget

**"Buy Within Budget" Mode**

When total > budget, this view hides lower-priority items automatically:

```
Budget: $150
Current Total: $178

SHOWN (Will buy):
  Must (15 items)      $112.00
  Should (8 items)     $38.00
  ────────────────────
  Total:              $150.00 ✓

HIDDEN (Over budget):
  Nice-to-have (6)     $28.00  [Hidden from view]
```

### View by Recipe

**Filter:** "Show Dinner items"

```
DINNER (4 recipes, 23 items, $67.00)

Recipe: Spaghetti Carbonara
  ☐ Spaghetti          $2.50
  ☐ Bacon              $5.00
  ☐ Eggs (have 2/4)    $1.20
  ☐ Parmesan           $4.00

Recipe: Chicken Stir-Fry
  ☐ Chicken thighs     $8.00
  ☐ Broccoli           $2.50
  ☐ Rice (have)        $0.00
  ...
```

---

## Budget Controls

### Setting Budgets

| Level | Description | Use Case |
|-------|-------------|----------|
| **Per List** | One budget for entire list | "Weekly groceries: $150" |
| **Per Store** | Budget per store | "Kroger: $100, Target: $50" |
| **Per Group** | Budget per tag | "Kids: $60, Home: $40" |
| **Per Person** | Individual spending limits | "John: $75, Sarah: $75" |

### Live Budget Awareness

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

**Price Sources:**
1. Last price paid (from receipts)
2. Manual estimates
3. Category defaults (e.g., "milk ~$4.50")
4. Product database (if exact item selected)

### Priority-Driven Decisions

Each item or group can be:
- **Must** - Essential, won't remove
- **Should** - Important but flexible
- **Nice-to-have** - Remove if over budget

### Over-Budget Intelligence

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

3. Remove 2 Should items (-$14.50)
   → Coffee, Cheese
   Still over by $13.50

[Apply Suggestion 1] [Customize] [Ignore]
```

### Forecasting

**Recurring Items Counted:**

```
Budget: $150/week
─────────────────────
Recurring (auto-added):
  Milk             $4.50
  Eggs             $3.00
  Bread            $2.50
  Coffee           $8.00
  ────────────────────
  Subtotal:       $18.00

Remaining for this week:  $132.00
```

**Future Forecast:**

```
Next 4 Weeks:
Week 1: $150 (on track)
Week 2: $165 (over by $15 - kids' clothes)
Week 3: $140 (under budget)
Week 4: $150 (on track)
```

---

## Shopping Modes

### In-Store Execution

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

**Design Features:**
- Large tap targets (thumb-friendly)
- Offline-friendly (syncs when connected)
- "Must items first" toggle
- One-hand operation
- Works in fluorescent lighting
- No thinking required

### Digital Shopping

**Integration Options:**

| Service | Feature |
|---------|---------|
| **Instacart** | Pre-build cart from list |
| **DoorDash** | Order from multiple stores |
| **Walmart** | Pickup or delivery |
| **Target** | Same-day delivery |
| **Uber** | Connect to local stores |

**Workflow:**

```
1. User clicks "Order Online"
2. Selects service (Instacart, etc.)
3. System exports list to service
4. User reviews/edits cart
5. Checks out through service
6. Delivery scheduled
```

---

## Receipt Scanning

### After Shopping

**Photo Upload:**
1. User uploads receipt photo
2. OCR parses line items
3. Matches items to list
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
  ✓ Butter        Est: $4.00  Act: $4.29
  ✓ Cheese        Est: $6.50  Act: $6.99

Unmatched Items (not on list):
  ? Bananas                   $1.50
  ? Yogurt                    $3.99

Budget vs Actual:
  Estimated:     $28.50
  Actual:        $30.94
  Difference:    +$2.44 (over by 8.6%)

Savings:
  You saved $1.12 with store card

[Confirm] [Edit Matches] [Add Missing Items]
```

### Price Learning

System automatically:
- Detects frequently bought items
- Improves future estimates
- Learns store-specific pricing
- Accounts for seasonal variation

---

## Group Buying

### Neighborhood Pooling

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

**Features:**

| Feature | Description |
|---------|-------------|
| **Pool Money** | Each household contributes budget |
| **Assign Items** | Who's buying what |
| **Split Costs** | Auto-calculate per household |
| **Track Contributions** | Who paid, who owes |
| **Approval Rules** | Optional approval for big items |

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

---

## Recurring Items

### Auto-Add Items

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

---

## Digital Shopping Integration

### Phase 1: Export List

```
Deep Links:
- "Open in Instacart"
- "Open in DoorDash"
- "Open in Uber"

System exports list cleanly to service.
```

### Phase 2: Product Matching

```
System pre-builds cart:
- Milk → "Kroger 2% Milk, 1 gal"
- Eggs → "Kroger Large Eggs, 12ct"
- Bread → "Kroger White Bread, 20oz"
```

### Phase 3: Full Checkout

```
Partnership with services:
- User checks out within app
- List remains source of truth
```

---

## User Management

### Roles

| Role | Permissions |
|------|-------------|
| **Owner** | Full control, delete list, manage members |
| **Editor** | Add/remove items, edit budget, mark purchased |
| **Viewer** | View list only, no edits |

### Household Management

```
Household: "The Johnsons"
├── Members
│   ├── Sarah (Owner)
│   ├── John (Editor)
│   └── Grandma (Viewer)
├── Default Budget: $150/week
├── Default Store: Kroger
└── Recurring Items: Milk, Eggs, Bread, Coffee
```

---

## Anti-Features (Intentionally Excluded)

| Feature | Why Not |
|---------|---------|
| **Social Feed** | Not Instagram for groceries |
| **Influencer Content** | No spam, no pressure |
| **Gamification** | No badges for buying stuff |
| **Ads** | Revenue from vendors, not users |
| **Complex UI** | Simple, fast, focused |

---

**One list. Every store. Real money control.**
