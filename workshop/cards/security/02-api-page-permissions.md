---
category: security
bcquality_domain: security
tag: opinion
---

# Does every API page need its own permission set, separate from the app's main one?

**Side A:** Yes. Integration users should get exactly the API surface and nothing more. Least privilege by default.

**Side B:** No. One well-scoped app permission set is easier to maintain. Separate sets multiply what admins have to understand.
