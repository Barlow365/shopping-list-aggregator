# ARCHITECTURE (IMPLEMENTATION NOTES)

| Entity | Description | Persisted |
| --- | --- | --- |
| ItemPool | Internal store of items | Yes |
| Item | Intended purchase | Yes |
| Tags | Item metadata | Yes |
| View | Generated query | No |
| Trip | Store/time execution | Yes |
| Receipt | Trip purchases | Yes |
| PriceMemory | Learned prices | Yes |
