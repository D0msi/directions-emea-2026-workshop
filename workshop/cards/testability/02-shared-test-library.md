---
category: testability
bcquality_domain: testing
tag: bites
seed: workshop/AmuseYou/RentalOperations.Test/src/UnitTests/AvailabilityTests.Codeunit.al
---

# Could this test helper be generalized into a shared library codeunit?

**Side A:** Yes. The same CreateTestEquipment helper exists in several test codeunits. One library codeunit means one place to fix when the table changes.

**Side B:** No. A local helper keeps each test codeunit readable on its own. A shared library couples unrelated test apps and grows into a god object.
