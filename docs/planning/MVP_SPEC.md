# MVP SPEC (DERIVED)

| Section | Content |
| --- | --- |
| Authority | Derived from PRODUCT_MAP + MASTER_PLAN |
| Goal | Validate item-first capture, tagging, and trip generation |
| Success | Users create items, generate trips, and enter receipts |

| Route | ESW Success Signal | XO Signal (if applicable) |
| --- | --- | --- |
| /app/intake | Item captured + tagged | Stepwise intake completes |
| /app/memory | Items reviewed and edited | Workspace shell used |
| /app/view | View created, trip started | Over-budget balance used |
| /app/trips/start | Trip generated | Store zone map used |
| /app/trips/:tripId | Items confirmed | Swipe flow used |
| /app/trips/:tripId/receipts | Receipt matched | Matching game used |

| ESW/XO Alignment | Rule |
| --- | --- |
| ESW | Defines routes, modes, states, actions |
| XO | Overlays only when bound to ESW route/state/action |
