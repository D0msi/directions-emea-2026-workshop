---
category: code-quality
bcquality_domain: style
tag: opinion
---

# Should a procedure signal failure by returning false, or by raising an error?

**Side A:** Raise an error. A Boolean nobody checks silently swallows the failure, and an error rolls back the transaction for you.

**Side B:** Return a Boolean when the caller has a sensible alternative path. Errors as control flow force callers into Codeunit.Run or TryFunctions.
