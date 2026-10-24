---
reviewed_with: Claude Code (Claude Sonnet 5.5, claude-sonnet-5-5)
reviewed_files: [workshop/AmuseYou/RentalOperations/src/BookingMgt.Codeunit.al, workshop/AmuseYou/RentalOperations/src/AvailabilityMgt.Codeunit.al, workshop/AmuseYou/RentalOperations/src/WeatherApiClient.Codeunit.al, workshop/AmuseYou/RentalOperations/src/BookingList.Page.al, workshop/AmuseYou/RentalOperations/src/RentalUpgrade.Codeunit.al, workshop/AmuseYou/BookingCore/src/BookingStatus.Enum.al, workshop/AmuseYou/BookingCore.Test/src/BookingTests.Codeunit.al, workshop/AmuseYou/RentalOperations.Test/src/UnitTests/AvailabilityTests.Codeunit.al, workshop/AmuseYou/RentalOperations.Test/src/UnitTests/BookingMgtTests.Codeunit.al, workshop/AmuseYou/Integration.Test/src/EndToEndBookingTests.Codeunit.al]
bcquality_commit: 57b3637
outcome: completed
blockers_found: 0
blockers: 0
---

# BCQuality Review: fallback-sonnet

Entry dispatched five leaf skills (al-style-review, al-testing-review, al-performance-review, al-security-review, al-upgrade-review), all `completed`. No custom rules exist in `custom/knowledge/`. `bc-version` was not supplied, so findings from articles restricted to newer BC versions are capped at medium confidence and name `bc-version` as unknown.

Counts: blocker 0, major 12, minor 14, info 0 (agent findings are included in minor).

## Blockers

None.

## Findings

- major · high · workshop/AmuseYou/RentalOperations/src/AvailabilityMgt.Codeunit.al:62 · `Commit()` inside the `UpdateMaintenanceStatus` record loop, with no persisted progress marker; each blocked equipment row becomes its own transaction and a failure part-way leaves a half-applied run. · microsoft/knowledge/performance/avoid-commit-inside-loops.md
- major · high · workshop/AmuseYou/RentalOperations/src/AvailabilityMgt.Codeunit.al:75 · `GetTotalRevenue` loops over completed bookings only to add the stored `Total Amount` field; the total should come from one `CalcSums` on the filtered source table. · microsoft/knowledge/performance/calcsums-instead-of-calcfields-in-loop.md
- major · high · workshop/AmuseYou/RentalOperations/src/BookingMgt.Codeunit.al:65 · `ProcessDailyBookings` assigns the constant `InProgress` and calls `Modify()` (no trigger) per row. `Booking` has no table triggers or subscribers in this repo, so `ModifyAll(Status, ...)` on the same filter is equivalent. · microsoft/knowledge/performance/prefer-modifyall-over-per-row-modify.md
- major · high · workshop/AmuseYou/RentalOperations/src/BookingMgt.Codeunit.al:65 · The same loop modifies every row but opens it with `FindSet()` rather than `FindSet(true)`, so the read takes a shared lock and `Modify` must upgrade it (deadlock candidate). · microsoft/knowledge/performance/findset-true-applies-updlock-on-read.md
- major · medium · workshop/AmuseYou/RentalOperations/src/WeatherApiClient.Codeunit.al:15 · The API key is concatenated into a plain-`Text` URL passed to `HttpClient.Get`; the secret-aware `HttpRequestMessage.SetSecretRequestUri` / `SecretText` path is not used. `bc-version` unknown; the article applies from BC 23. Also supported by microsoft/knowledge/security/secrettext-for-credentials.md. · microsoft/knowledge/security/secrettext-with-httpclient.md
- major · medium · workshop/AmuseYou/RentalOperations/src/WeatherApiClient.Codeunit.al:37 · Same pattern in `CheckSevereWeather`: the API key is concatenated into the alerts URL and sent through `HttpClient.Get`. `bc-version` unknown (article applies from BC 23). · microsoft/knowledge/security/secrettext-with-httpclient.md
- major · high · workshop/AmuseYou/Integration.Test/src/EndToEndBookingTests.Codeunit.al:96 · `asserterror BookingMgt.CreateBooking(Booking2)` is followed only by a `GetLastErrorText() <> ''` check, so any error (for example a missing setup record) makes the test pass; pin it with `Assert.ExpectedError` or `Assert.ExpectedErrorCode`. · microsoft/knowledge/testing/asserterror-needs-expectederror-and-code.md
- major · high · workshop/AmuseYou/BookingCore.Test/src/BookingTests.Codeunit.al:19 · Primary-key fixture value hardcoded as `'BOOK001'` (also `'BOOK002'` at line 39); it collides with existing data or repeated runs. Use a `LibraryUtility` helper. · microsoft/knowledge/testing/use-generateguid-for-unique-test-fixture-values.md
- major · high · workshop/AmuseYou/RentalOperations.Test/src/UnitTests/AvailabilityTests.Codeunit.al:21 · Equipment and booking keys hardcoded (`'AVAIL001'`, `'AVAIL002'`, `'BOUNCE001'..'BOUNCE003'`, `'BOOK001'` reused in two tests). · microsoft/knowledge/testing/use-generateguid-for-unique-test-fixture-values.md
- major · high · workshop/AmuseYou/RentalOperations.Test/src/UnitTests/BookingMgtTests.Codeunit.al:25 · Primary-key fixture values hardcoded (`'BOOK001'`, `'CALC001'`, `'CANCEL001'`). · microsoft/knowledge/testing/use-generateguid-for-unique-test-fixture-values.md
- major · high · workshop/AmuseYou/Integration.Test/src/EndToEndBookingTests.Codeunit.al:33 · Primary-key fixture values hardcoded (`'E2EBOOK001'`, `'E2E001'`, `'FIRST001'`, `'SECOND001'`, `'CONCURRENT001'`). · microsoft/knowledge/testing/use-generateguid-for-unique-test-fixture-values.md
- major · high · workshop/AmuseYou/RentalOperations/src/RentalUpgrade.Codeunit.al:15 · `ModifyAll(Location, 'MAIN')` is implemented inline in `OnUpgradePerCompany`; the trigger should only call a named helper per feature. · microsoft/knowledge/upgrade/triggers-call-helpers-not-implementations.md
- minor · medium · workshop/AmuseYou/RentalOperations/src/WeatherApiClient.Codeunit.al:6 · A live-looking API key is hardcoded in source as a Locked label instead of being stored in `IsolatedStorage` (via `SetEncrypted`). · microsoft/knowledge/security/secrets-isolated-storage.md
- minor · medium · workshop/AmuseYou/RentalOperations/src/WeatherApiClient.Codeunit.al:46 · The weather response comes from an endpoint called with no `Authorization` header, OAuth token or certificate. Its `severeWeather` flag decides availability with no size cap, no type check and no check that the response matches the requested location and date. · microsoft/knowledge/security/validate-unauthenticated-response-before-use.md
- minor · high · workshop/AmuseYou/RentalOperations/src/BookingMgt.Codeunit.al:13 · `Error('Booking number must be specified.')` uses a hardcoded literal instead of a Label with an `Err` suffix, so the text is not translatable. · microsoft/knowledge/style/error-passes-parameters-directly-not-strsubstno.md
- minor · high · workshop/AmuseYou/RentalOperations/src/BookingMgt.Codeunit.al:16 · `Error('Equipment %1 is not available ...', ...)` has the same problem: the format string is a literal rather than a Label. · microsoft/knowledge/style/error-passes-parameters-directly-not-strsubstno.md
- minor · high · workshop/AmuseYou/RentalOperations/src/BookingList.Page.al:80 · `Message('Booking amounts recalculated.')` uses a literal instead of a Label; the article applies the same rule to `Message`. · microsoft/knowledge/style/error-passes-parameters-directly-not-strsubstno.md
- minor · medium · workshop/AmuseYou/BookingCore.Test/src/BookingTests.Codeunit.al:13 · No `[FEATURE]` or `[SCENARIO]` tags; only GIVEN/WHEN/THEN comments are present. · microsoft/knowledge/testing/test-feature-scenario-tags.md
- minor · medium · workshop/AmuseYou/RentalOperations.Test/src/UnitTests/AvailabilityTests.Codeunit.al:14 · No `[FEATURE]` or `[SCENARIO]` tags on the three tests. · microsoft/knowledge/testing/test-feature-scenario-tags.md
- minor · medium · workshop/AmuseYou/RentalOperations.Test/src/UnitTests/BookingMgtTests.Codeunit.al:14 · No `[FEATURE]` or `[SCENARIO]` tags on the two tests. · microsoft/knowledge/testing/test-feature-scenario-tags.md
- minor · medium · workshop/AmuseYou/Integration.Test/src/EndToEndBookingTests.Codeunit.al:6 · `[SCENARIO]` comments are present, but there is no `[FEATURE]` tag naming the functional area for the codeunit. · microsoft/knowledge/testing/test-feature-scenario-tags.md
- minor · medium · workshop/AmuseYou/RentalOperations/src/RentalUpgrade.Codeunit.al:9 · The upgrade trigger has no `HasUpgradeTag` guard and no `SetUpgradeTag`, so the step reruns on every upgrade instead of once per company. `ModifyAll` with a blank-location filter is idempotent, which limits the impact. · microsoft/knowledge/upgrade/use-upgrade-tags-not-version-checks.md
- minor · medium · workshop/AmuseYou/RentalOperations/src/RentalUpgrade.Codeunit.al:15 · Back-fills the new `Location` field on an existing table with `ModifyAll`; the article prefers `DataTransfer` with `AddConstantValue` for new fields on existing tables. `bc-version` unknown (article applies from BC 21); table size unknown. · microsoft/knowledge/upgrade/datatransfer-for-bulk-init.md
- minor · medium · workshop/AmuseYou/RentalOperations.Test/src/UnitTests/AvailabilityTests.Codeunit.al:61 · agent finding (`agent:testing-wrong-record-type-argument`). `CreateTestBooking(Equipment1, ...)` passes an `Equipment` record to a `var Booking: Record Booking` parameter, which will not compile. Pass a `Booking` record variable instead.
- minor · medium · workshop/AmuseYou/RentalOperations.Test/src/UnitTests/AvailabilityTests.Codeunit.al:24 · agent finding (`agent:testing-unmocked-http-call`). Tests that use `BounceUnit` equipment (`IsEquipmentAvailable` in AvailabilityTests, `CreateBooking` in BookingMgtTests, the end-to-end workflow test) reach `WeatherApiClient.CheckSevereWeather` and make a real outbound HTTP call. Results depend on network and weather, and an unreachable service silently makes the check pass. Give the weather check a test seam or use a non-outdoor equipment type.

Suppressed: none. There are no custom rules and no layer-precedence conflicts.

## Verdicts

Not triaged yet.
