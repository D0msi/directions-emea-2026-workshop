---
category: upgradeability
bcquality_domain: upgrade
tag: opinion
---

# Should upgrade code tolerate bad data and carry on, or stop the upgrade?

**Side A:** Tolerate and log. A blocked upgrade blocks the whole tenant over one bad row.

**Side B:** Stop. Upgrading on top of bad data spreads it. Better to fail loudly while someone is watching.
