---
reviewed_with: "Claude Code (Claude Opus 5.5, claude-opus-5-5)"
reviewed_files: [workshop/AmuseYou/RentalOperations/src/BookingMgt.Codeunit.al, workshop/AmuseYou/RentalOperations/src/AvailabilityMgt.Codeunit.al, workshop/AmuseYou/RentalOperations/src/WeatherApiClient.Codeunit.al, workshop/AmuseYou/RentalOperations/src/BookingList.Page.al, workshop/AmuseYou/RentalOperations/src/RentalUpgrade.Codeunit.al, workshop/AmuseYou/BookingCore/src/BookingStatus.Enum.al, workshop/AmuseYou/BookingCore.Test/src/BookingTests.Codeunit.al, workshop/AmuseYou/RentalOperations.Test/src/UnitTests/AvailabilityTests.Codeunit.al, workshop/AmuseYou/RentalOperations.Test/src/UnitTests/BookingMgtTests.Codeunit.al, workshop/AmuseYou/Integration.Test/src/EndToEndBookingTests.Codeunit.al]
bcquality_commit: 57b3637
outcome: completed
blockers_found: 1
blockers: 1
---

# BCQuality Review: fallback-opus

## Blockers

- `microsoft/knowledge/testing/asserterror-needs-expectederror-and-code.md` · workshop/AmuseYou/Integration.Test/src/EndToEndBookingTests.Codeunit.al:95 · Bare `asserterror` followed by `GetLastErrorText() <> ''` (always true), so any error makes TestConcurrentBookingPrevention pass; assert the specific error with Assert.ExpectedError. · microsoft/knowledge/testing/asserterror-needs-expectederror-and-code.md

## Findings

- major · high · workshop/AmuseYou/RentalOperations/src/WeatherApiClient.Codeunit.al:15 · GetWeatherForecast concatenates the API key (plain Text label) into the URL sent via HttpClient.Get; use SecretText + SecretStrSubstNo + SetSecretRequestUri. · microsoft/knowledge/security/secrettext-with-httpclient.md
- major · high · workshop/AmuseYou/RentalOperations/src/WeatherApiClient.Codeunit.al:37 · CheckSevereWeather concatenates the API key into the alerts URL sent via HttpClient.Get; same SecretText fix. · microsoft/knowledge/security/secrettext-with-httpclient.md
- major · high · workshop/AmuseYou/RentalOperations/src/AvailabilityMgt.Codeunit.al:62 · UpdateMaintenanceStatus calls Commit() inside the record loop with no persisted progress marker; not atomic and not restart-safe. · microsoft/knowledge/performance/avoid-commit-inside-loops.md
- major · high · workshop/AmuseYou/RentalOperations/src/AvailabilityMgt.Codeunit.al:57 · UpdateMaintenanceStatus reads (and update-locks) every Equipment row and tests "Last Maintenance Date" inside the loop instead of SetFilter before FindSet. · microsoft/knowledge/performance/apply-filters-before-iterating.md
- major · high · workshop/AmuseYou/RentalOperations/src/AvailabilityMgt.Codeunit.al:75 · GetTotalRevenue loops Completed bookings only to sum the stored "Total Amount"; use CalcSums. · microsoft/knowledge/performance/calcsums-instead-of-calcfields-in-loop.md
- major · high · workshop/AmuseYou/RentalOperations/src/BookingMgt.Codeunit.al:65 · ProcessDailyBookings uses FindSet() (no update lock) and then Modify on every row; use FindSet(true). · microsoft/knowledge/performance/findset-true-applies-updlock-on-read.md
- major · high · workshop/AmuseYou/RentalOperations/src/BookingMgt.Codeunit.al:65 · ProcessDailyBookings assigns a constant status per row with Modify() and no per-row logic; one ModifyAll on the filtered set is equivalent. · microsoft/knowledge/performance/prefer-modifyall-over-per-row-modify.md
- major · high · workshop/AmuseYou/RentalOperations/src/RentalUpgrade.Codeunit.al:9 · OnUpgradePerCompany implements the data change inline instead of calling a named local upgrade procedure. · microsoft/knowledge/upgrade/triggers-call-helpers-not-implementations.md
- major · medium · workshop/AmuseYou/BookingCore.Test/src/BookingTests.Codeunit.al:50 · CreateTestBooking hand-rolls Init/Insert, references Equipment 'TEST001' that is never created, and bypasses field validation; use a shared test-library helper. · microsoft/knowledge/testing/use-library-codeunits-for-test-fixtures.md
- major · medium · workshop/AmuseYou/RentalOperations.Test/src/UnitTests/BookingMgtTests.Codeunit.al:68 · CreateTestBooking hand-rolls Init/Insert against non-existent Equipment 'TEST001' and bypasses validation. · microsoft/knowledge/testing/use-library-codeunits-for-test-fixtures.md
- major · medium · workshop/AmuseYou/RentalOperations.Test/src/UnitTests/AvailabilityTests.Codeunit.al:40 · Hardcoded fixture keys ('BOOK001' inserted by two tests in one codeunit; same pattern in all four test codeunits); generate keys with Library - Utility. Runner TestIsolation unknown. · microsoft/knowledge/testing/use-generateguid-for-unique-test-fixture-values.md
- minor · high · workshop/AmuseYou/RentalOperations/src/WeatherApiClient.Codeunit.al:6 · Weather API key is hard-coded in source as a Locked Label; persist it in IsolatedStorage and read as SecretText. · microsoft/knowledge/security/secrets-isolated-storage.md
- minor · high · workshop/AmuseYou/RentalOperations/src/RentalUpgrade.Codeunit.al:9 · v1.1 location migration has no upgrade tag (HasUpgradeTag/SetUpgradeTag), so it re-runs on every upgrade and can overwrite intentionally blank locations. · microsoft/knowledge/upgrade/use-upgrade-tags-not-version-checks.md
- minor · high · workshop/AmuseYou/RentalOperations/src/BookingMgt.Codeunit.al:13 · Error called with a hard-coded literal instead of an Err Label. · microsoft/knowledge/style/error-passes-parameters-directly-not-strsubstno.md
- minor · high · workshop/AmuseYou/RentalOperations/src/BookingMgt.Codeunit.al:16 · Error called with a hard-coded format literal instead of an Err Label with placeholder Comment. · microsoft/knowledge/style/error-passes-parameters-directly-not-strsubstno.md
- minor · high · workshop/AmuseYou/RentalOperations/src/BookingList.Page.al:80 · Message called with a hard-coded literal instead of a Label. · microsoft/knowledge/style/error-passes-parameters-directly-not-strsubstno.md
- minor · high · workshop/AmuseYou/BookingCore.Test/src/BookingTests.Codeunit.al:5 · No [FEATURE] on the codeunit and no [SCENARIO] in its tests. · microsoft/knowledge/testing/test-feature-scenario-tags.md
- minor · high · workshop/AmuseYou/RentalOperations.Test/src/UnitTests/AvailabilityTests.Codeunit.al:6 · No [FEATURE] on the codeunit and no [SCENARIO] in its tests. · microsoft/knowledge/testing/test-feature-scenario-tags.md
- minor · high · workshop/AmuseYou/RentalOperations.Test/src/UnitTests/BookingMgtTests.Codeunit.al:6 · No [FEATURE] on the codeunit and no [SCENARIO] in its tests. · microsoft/knowledge/testing/test-feature-scenario-tags.md
- minor · high · workshop/AmuseYou/Integration.Test/src/EndToEndBookingTests.Codeunit.al:6 · No [FEATURE] comment on the codeunit. · microsoft/knowledge/testing/test-feature-scenario-tags.md
- minor · medium · workshop/AmuseYou/RentalOperations/src/BookingList.Page.al:22 · Field tooltips are the mechanical "Specifies the <field>." form and add nothing beyond the caption. · microsoft/knowledge/style/tooltip-required-on-page-fields.md
- minor · medium · workshop/AmuseYou/Integration.Test/src/EndToEndBookingTests.Codeunit.al:14 · TestCompleteBookingWorkflow has four [WHEN] blocks whose assertions decompose per action; split or reshape as a flow test. · microsoft/knowledge/testing/test-one-when-per-test.md
- minor · medium · workshop/AmuseYou/BookingCore/src/BookingStatus.Enum.al:4 · XML summary describes values (Active, 24-hour rule) that do not match the enum; rewrite it. · agent finding
- minor · medium · workshop/AmuseYou/RentalOperations.Test/src/UnitTests/AvailabilityTests.Codeunit.al:61 · Equipment1 (Record Equipment) passed as `var Booking: Record Booking`; the test app would not compile (real impact blocker-level; promote to a rule before it can gate). · agent finding
- minor · medium · workshop/AmuseYou/RentalOperations.Test/src/UnitTests/AvailabilityTests.Codeunit.al:24 · Tests on BounceUnit equipment trigger a live HttpClient call to the weather API; make the weather check substitutable. · agent finding

Suppressed: None (no custom rules in `custom/knowledge/`; no layer-precedence conflicts).

## Verdicts

Not triaged yet.
