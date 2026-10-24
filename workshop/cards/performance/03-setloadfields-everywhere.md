---
category: performance
bcquality_domain: performance
tag: opinion
---

# Should SetLoadFields be used on every read, or only on measured hot paths?

**Side A:** Everywhere it applies. It's cheap to write, and the next developer won't measure before the table grows.

**Side B:** Only where measured. On small tables it's noise, and it causes JIT-load surprises when someone later reads a field that wasn't loaded.
