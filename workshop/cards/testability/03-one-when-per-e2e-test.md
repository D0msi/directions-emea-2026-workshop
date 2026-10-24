---
category: testability
bcquality_domain: testing
tag: bites
seed: workshop/AmuseYou/Integration.Test/src/EndToEndBookingTests.Codeunit.al
---

# Does "one WHEN per test" apply to end-to-end tests too?

**Side A:** Yes. A test with four WHENs fails at step two and tells you nothing about steps three and four. Split it.

**Side B:** No. An end-to-end test exists to prove the lifecycle works in sequence. Splitting it means rebuilding the state four times and testing something else.
