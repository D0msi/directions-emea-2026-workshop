---
category: performance
bcquality_domain: performance
tag: bites
seed: workshop/AmuseYou/RentalOperations/src/AvailabilityMgt.Codeunit.al
---

# A change that slows a routine by 250%: bug or documented trade-off?

**Side A:** Bug. A routine called inside loops and lookups can't get 3.5x slower without a design change (cache it, move it to a background task).

**Side B:** Trade-off. It's documented in the code, it's for customer safety, and correctness beats speed. Reviews should respect documented decisions.
