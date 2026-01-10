# PAGES & COMPONENTS PLAN (IMPLEMENTATION NOTES)

| Page | Reads | Writes | Components |
| --- | --- | --- | --- |
| /app/intake | Item Pool | Items, tags | AID, TagPrompts |
| /app/memory | Item Pool, Price Memory | Items, tags | ItemList, TagEditor |
| /app/view | Item Pool | View params | ViewBuilder |
| /app/trips/start | Item Pool | Trip | TripBuilder |
| /app/trips/:tripId | Trip | Trip state | ShopMode |
| /app/trips/:tripId/receipts | Trip | Receipt | ReceiptMatcher |
