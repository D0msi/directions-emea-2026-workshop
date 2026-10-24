---
category: performance
bcquality_domain: performance
tag: opinion
---

# Should a long-running batch job commit per record, or run as one transaction?

**Side A:** Per record (or per batch). Progress survives a failure and locks are released early, so users aren't blocked for the whole run.

**Side B:** One transaction. All-or-nothing is easier to reason about, and partially processed data is a support case waiting to happen.
