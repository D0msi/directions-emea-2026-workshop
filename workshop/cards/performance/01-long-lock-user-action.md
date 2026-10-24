---
category: performance
bcquality_domain: performance
tag: bites
seed: workshop/AmuseYou/RentalOperations/src/BookingMgt.Codeunit.al
---

# Is a long lock on a key table ever acceptable in a user-triggered action?

**Side A:** No. One user clicking "Recalculate Amounts" must never block everyone else's bookings. Move it to a job queue or lock less.

**Side B:** Yes, when consistency matters more than concurrency and the action is rare and run by an admin. A half-recalculated price list is worse than a short wait.
