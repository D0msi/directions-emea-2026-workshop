---
category: performance
bcquality_domain: performance
tag: opinion
---

# Is a FlowField acceptable on a list page with thousands of rows?

**Side A:** No. Every visible row costs a calculation. Store the value or move it to a FactBox.

**Side B:** Yes, with proper SumIndexFields. Stored copies drift out of sync, and that bug is worse than a slightly slower list.
