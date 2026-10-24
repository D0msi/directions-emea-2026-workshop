---
category: code-quality
bcquality_domain: style
tag: opinion
---

# Should a code review report issues the compiler or code analyzers already flag?

**Side A:** No. Analyzers are deterministic and already gate the build. Repeating them in review is noise that hides the real findings.

**Side B:** Yes. Many teams suppress or ignore analyzer warnings. If the review is the only place a human reads it, it should say so.
