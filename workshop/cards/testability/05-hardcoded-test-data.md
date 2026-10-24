---
category: testability
bcquality_domain: testing
tag: opinion
---

# Should test data use readable hardcoded values ('BOOK001') or generated ones?

**Side A:** Generated. Hardcoded keys collide with existing data and between tests, and hide assumptions about what the data looks like.

**Side B:** Hardcoded. When a test fails, 'BOOK001' tells you exactly what happened. Random data makes failures hard to reproduce.
