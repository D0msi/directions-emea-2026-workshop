---
category: code-quality
bcquality_domain: style
tag: opinion
---

# Should a codeunit implement an interface before a second implementation exists?

**Side A:** Yes. The interface is the contract that lets tests and partners swap the implementation. Adding it later is a breaking change.

**Side B:** No. One implementation behind an interface is ceremony. Introduce the interface when the second implementation actually exists.
