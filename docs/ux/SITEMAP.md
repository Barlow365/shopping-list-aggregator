```text
+==============================================================================+
| THE ONE LIST SYSTEM | ROUTE MAP (BOUND TO PRODUCT MAP)                        |
| Capture | Decide | Shop | Close Loop | Scale Up                               |
+==============================================================================+
| NODE (PRODUCT_MAP)                     | ROUTES                              |
+----------------------------------------+-------------------------------------+
| ENTRY > Public                          | /  /how-it-works  /features         |
|                                         | /pricing  /privacy  /terms           |
| ENTRY > Auth                            | /login  /signup  /onboarding         |
|                                         | onboarding: step rail + live preview |
+----------------------------------------+-------------------------------------+
| CORE OBJECT > Home                      | /app/home                            |
| CORE OBJECT > Groups                    | /app/groups  /app/groups/:groupId    |
+----------------------------------------+-------------------------------------+
| CAPTURE > Items                         | /app/lists/:listId/items             |
| CAPTURE > Voice                         | /app/lists/:listId/voice             |
| CAPTURE > Recipes                       | /app/lists/:listId/recipes           |
+----------------------------------------+-------------------------------------+
| DECIDE > Budget                         | /app/lists/:listId/budget            |
+----------------------------------------+-------------------------------------+
| SHOP > Stores                           | /app/lists/:listId/stores            |
| SHOP > Shop Mode                        | /app/lists/:listId/shop              |
| SHOP > Delivery                         | /app/lists/:listId/delivery          |
+----------------------------------------+-------------------------------------+
| CLOSE LOOP > Receipts                   | /app/lists/:listId/receipts          |
| CLOSE LOOP > History                    | /app/lists/:listId/history           |
+----------------------------------------+-------------------------------------+
| SCALE UP > Recurring                    | /app/lists/:listId/recurring         |
| SCALE UP > Pooling                      | /app/lists/:listId/pooling           |
| SCALE UP > Controls                     | /app/lists/:listId/controls          |
| SCALE UP > Upgrades                     | /app/lists/:listId/upgrades          |
+----------------------------------------+-------------------------------------+
| SYSTEM                                 | /app/settings  /app/help  /system    |
+==============================================================================+
```
