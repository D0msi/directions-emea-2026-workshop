---
category: code-quality
bcquality_domain: style
tag: opinion
---

# Must every user-facing string be a Label, even an error only a developer will ever see?

**Side A:** Yes. Any string that reaches the UI can reach a user and must be translatable. No exceptions means no judgement calls.

**Side B:** No. Internal guard errors ("should never happen") don't need translation overhead. Labels are for messages a user is expected to act on.
