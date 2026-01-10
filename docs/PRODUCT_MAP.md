```text
+==============================================================================+
| THE ONE LIST SYSTEM | PRODUCT MAP (CANONICAL)                                |
| Capture | Decide | Shop | Close Loop | Scale Up                              |
+==============================================================================+
| HEADER                                                                       |
| App Home | Active List: Weekly Groceries | Sync: Online | Profile            |
+==============================================================================+
| STATUS STRIP                                                                 |
| Budget: $150  Est: $128  Delta: +$22  Stores: 3  Must: 5                     |
+==============================================================================+
| DESIGN TOKENS (FOR FIGMA)                                                |
| Palette                                                                      |
|  Ink:      #111111   Paper:  #F5F2EC   Line:   #D6D2C4                        |
|  Accent:   #F2B705   Teal:   #0E7C7B   Danger: #C0392B                        |
|  Success:  #2E7D32   Info:   #1F6FEB   Muted:  #6B7280                        |
| Type                                                                         |
|  Display:  Suisse Intl (or similar)                                          |
|  Mono:     IBM Plex Mono                                                     |
| Buttons                                                                      |
|  Primary: Accent fill, Ink text                                              |
|  Secondary: Line border, Ink text                                            |
|  Ghost: text only, Teal on hover                                             |
|  Danger: Danger fill, Paper text                                             |
| States                                                                       |
|  Active tab: Teal underline                                                  |
|  Disabled: Muted text, Line border                                            |
+==============================================================================+
| ENTRY                                      | CORE OBJECT (LIST)              |
| Public                                     | List: Weekly Groceries          |
|  /            Home                         | Members: Sam, Alex, Priya        |
|  /how-it-works  Capture -> Decide -> Shop  | Active: Yes (1 active only)     |
|  /features      Shared | Budget | Multi    | Archive: read-only past lists   |
|  /pricing       Free during MVP            |                                 |
|  /privacy /terms                            |                                 |
| Auth                                       | MODES (ALL LENSES ON SAME LIST)  |
|  /login /signup /onboarding                | Items | Stores | Budget | Shop   |
|   Onboarding: step rail + live preview     | Receipts | History | Voice       |
|   Steps: group -> list -> budget -> invite | Recipes | Recurring | Delivery   |
|                                            | Pooling | Controls | Upgrades    |
+--------------------------------------------+---------------------------------+
| CAPTURE (LIST LENS)                         | DECIDE (LIST LENS)              |
| Items /app/lists/:listId/items  [ACTIVE]   | Budget /app/lists/:listId/budget |
|  - Quick add: "milk, eggs, bread"          |  - Budget set/edit               |
|  - Auto-split on comma                      |  - Running total vs budget       |
|  - Fields: name, qty, priority, store       |  - Over-budget banner            |
|  - Added-by attribution                     |  - Actions: remove nice-to-have  |
|  - Bulk tools: assign store, set priority   |  - Price memory panel            |
| Voice /app/lists/:listId/voice (stub)       | Priorities (inline on items)     |
|  - UI placeholder                           |  - Must / Should / Nice-to-have  |
|  - Fallback: paste text input               |                                 |
| Recipes /app/lists/:listId/recipes (manual) |                                 |
|  - Manual recipe card                       |                                 |
|  - Add ingredients to list                  |                                 |
+--------------------------------------------+---------------------------------+
| SHOP (LIST LENS)                            | CLOSE LOOP (LIST LENS)           |
| Stores /app/lists/:listId/stores [ACTIVE]   | Receipts /app/lists/:listId/receipts [ACTIVE] |
|  - Store chips: Kroger | Target | Home      |  - Upload receipt photo          |
|  - Unassigned bucket                        |  - Manual match + actual prices  |
|  - Bulk assign to store                     |  - Budgeted vs Actual            |
| Shop Mode /app/lists/:listId/shop [ACTIVE]  |  - Update last price paid        |
|  - Choose store (or All)                    | History /app/lists/:listId/history (stub) |
|  - Full-screen checklist                    |  - Activity timeline             |
|  - Big taps, offline checkoff               |  - Price memory table            |
| Delivery /app/lists/:listId/delivery (stub) |                                 |
|  - Export list now                           |                                 |
+--------------------------------------------+---------------------------------+
| SCALE UP (PHASED, WIRED)                    | SYSTEM HEALTH                    |
| Recurring /app/lists/:listId/recurring      | Offline banner                   |
|  - Rules stub                               | Sync conflicts                   |
|  - Forecast stub                            | Error states                     |
| Pooling /app/lists/:listId/pooling          |                                 |
|  - Pool money stub                          | DANGER ZONE                      |
|  - Assign buyers stub                       | Archive List                     |
| Controls /app/lists/:listId/controls        | Delete List                      |
|  - Caps per person                           | Leave Group                      |
|  - Expensive item gate                      |                                 |
| Upgrades /app/lists/:listId/upgrades        |                                 |
|  - Enable Voice | Recipes | Delivery | Pooling | Controls                     |
+==============================================================================+
```
