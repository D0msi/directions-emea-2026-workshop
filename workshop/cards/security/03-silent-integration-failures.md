---
category: security
bcquality_domain: security
tag: opinion
---

# Should an outbound integration call fail silently (return empty) or surface an error?

**Side A:** Surface it. Silent failure hides outages and can turn a safety check into "always OK".

**Side B:** Fail silently. A weather service being down must not stop people from booking. Log it and carry on.
