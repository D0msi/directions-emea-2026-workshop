---
category: testability
bcquality_domain: testing
tag: opinion
---

# Do tests with wall-clock asserts ("completes in under 5 seconds") belong in the regular test suite?

**Side A:** No. Timing depends on the machine and its load. A flaky test teaches the team to ignore red builds.

**Side B:** Yes. Without a guard in the suite, performance regressions ship. A generous threshold catches the 10x mistakes without flaking.
