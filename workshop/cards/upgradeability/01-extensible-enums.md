---
category: upgradeability
bcquality_domain: upgrade
tag: bites
seed: workshop/AmuseYou/BookingCore/src/BookingStatus.Enum.al
---

# Should every enum be Extensible = true by default?

**Side A:** Yes. Partners will need to extend it, and an enum you forgot to open can't be extended without a new release of your app.

**Side B:** No. A status enum drives case statements and business rules. An extension value nobody's code handles breaks invariants. Extensible should be a decision.
