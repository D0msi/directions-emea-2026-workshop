---
reviewed_with: Claude Code (Claude Sonnet 5.5)
reviewed_files: [workshop/AmuseYou/RentalOperations/src/BookingMgt.Codeunit.al, workshop/AmuseYou/RentalOperations/src/AvailabilityMgt.Codeunit.al, workshop/AmuseYou/RentalOperations/src/WeatherApiClient.Codeunit.al, workshop/AmuseYou/RentalOperations/src/BookingList.Page.al, workshop/AmuseYou/RentalOperations/src/RentalUpgrade.Codeunit.al, workshop/AmuseYou/BookingCore/src/BookingStatus.Enum.al, workshop/AmuseYou/BookingCore.Test/src/BookingTests.Codeunit.al, workshop/AmuseYou/RentalOperations.Test/src/UnitTests/AvailabilityTests.Codeunit.al, workshop/AmuseYou/RentalOperations.Test/src/UnitTests/BookingMgtTests.Codeunit.al, workshop/AmuseYou/Integration.Test/src/EndToEndBookingTests.Codeunit.al]
bcquality_commit: 1c963f1
outcome: completed
blockers_found: 0
blockers: 0
---

# BCQuality Review: fallback-lab2

## Blockers

None.

## Findings

- major · high · workshop/AmuseYou/RentalOperations/src/BookingMgt.Codeunit.al:78 · RecalculateAllAmounts calls LockTable and modifies all open bookings while locked; it is called directly from the Recalculate Amounts page action (BookingList.Page.al:79) · custom/knowledge/performance/no-table-locks-in-user-actions.md
- major · medium · workshop/AmuseYou/RentalOperations/src/AvailabilityMgt.Codeunit.al:62 · Commit inside the repeat loop after every blocked Equipment row · microsoft/knowledge/performance/avoid-commit-inside-loops.md
- major · medium · workshop/AmuseYou/RentalOperations/src/AvailabilityMgt.Codeunit.al:59 · Unfiltered FindSet(true) over all Equipment with the Last Maintenance Date test inside the loop · microsoft/knowledge/performance/apply-filters-before-iterating.md
- major · medium · workshop/AmuseYou/RentalOperations/src/AvailabilityMgt.Codeunit.al:73 · GetTotalRevenue loops only to sum the stored Total Amount; use CalcSums · microsoft/knowledge/performance/calcsums-instead-of-calcfields-in-loop.md
- major · high · workshop/AmuseYou/RentalOperations/src/WeatherApiClient.Codeunit.al:15 · API key concatenated as plain Text into the forecast URL and sent with HttpClient.Get; keep it SecretText (SecretStrSubstNo, SetSecretRequestUri, Send) · microsoft/knowledge/security/secrettext-with-httpclient.md
- major · high · workshop/AmuseYou/RentalOperations/src/WeatherApiClient.Codeunit.al:37 · API key concatenated as plain Text into the alerts URL and sent with HttpClient.Get · microsoft/knowledge/security/secrettext-with-httpclient.md
- major · medium · workshop/AmuseYou/RentalOperations/src/RentalUpgrade.Codeunit.al:14 · OnUpgradePerCompany does the work inline (SetRange + ModifyAll) instead of calling a named helper · microsoft/knowledge/upgrade/triggers-call-helpers-not-implementations.md
- major · medium · workshop/AmuseYou/Integration.Test/src/EndToEndBookingTests.Codeunit.al:96 · asserterror not followed by ExpectedError or ExpectedErrorCode; test only checks GetLastErrorText() differs from empty · microsoft/knowledge/testing/asserterror-needs-expectederror-and-code.md
- major · medium · workshop/AmuseYou/BookingCore.Test/src/BookingTests.Codeunit.al:19 · Hardcoded primary key Booking No. 'BOOK001' · microsoft/knowledge/testing/use-generateguid-for-unique-test-fixture-values.md
- major · medium · workshop/AmuseYou/BookingCore.Test/src/BookingTests.Codeunit.al:39 · Hardcoded primary key Booking No. 'BOOK002' · microsoft/knowledge/testing/use-generateguid-for-unique-test-fixture-values.md
- major · medium · workshop/AmuseYou/RentalOperations.Test/src/UnitTests/AvailabilityTests.Codeunit.al:21 · Hardcoded primary key Equipment No. 'AVAIL001' · microsoft/knowledge/testing/use-generateguid-for-unique-test-fixture-values.md
- major · medium · workshop/AmuseYou/RentalOperations.Test/src/UnitTests/AvailabilityTests.Codeunit.al:39 · Hardcoded primary key Equipment No. 'AVAIL002' · microsoft/knowledge/testing/use-generateguid-for-unique-test-fixture-values.md
- major · medium · workshop/AmuseYou/RentalOperations.Test/src/UnitTests/AvailabilityTests.Codeunit.al:40 · Hardcoded primary key Booking No. 'BOOK001' (reused at line 61 and in other codeunits) · microsoft/knowledge/testing/use-generateguid-for-unique-test-fixture-values.md
- major · medium · workshop/AmuseYou/RentalOperations.Test/src/UnitTests/AvailabilityTests.Codeunit.al:57 · Hardcoded Equipment No. 'BOUNCE001' to 'BOUNCE003', also asserted by count · microsoft/knowledge/testing/use-generateguid-for-unique-test-fixture-values.md
- major · medium · workshop/AmuseYou/RentalOperations.Test/src/UnitTests/AvailabilityTests.Codeunit.al:61 · Hardcoded primary key Booking No. 'BOOK001', same as line 40 · microsoft/knowledge/testing/use-generateguid-for-unique-test-fixture-values.md
- major · medium · workshop/AmuseYou/RentalOperations.Test/src/UnitTests/BookingMgtTests.Codeunit.al:21 · Hardcoded primary key Equipment No. 'CALC001' · microsoft/knowledge/testing/use-generateguid-for-unique-test-fixture-values.md
- major · medium · workshop/AmuseYou/RentalOperations.Test/src/UnitTests/BookingMgtTests.Codeunit.al:25 · Hardcoded primary key Booking No. 'BOOK001' · microsoft/knowledge/testing/use-generateguid-for-unique-test-fixture-values.md
- major · medium · workshop/AmuseYou/RentalOperations.Test/src/UnitTests/BookingMgtTests.Codeunit.al:47 · Hardcoded primary key Booking No. 'CANCEL001' · microsoft/knowledge/testing/use-generateguid-for-unique-test-fixture-values.md
- major · medium · workshop/AmuseYou/Integration.Test/src/EndToEndBookingTests.Codeunit.al:25 · Hardcoded primary key Equipment No. 'E2E001' · microsoft/knowledge/testing/use-generateguid-for-unique-test-fixture-values.md
- major · medium · workshop/AmuseYou/Integration.Test/src/EndToEndBookingTests.Codeunit.al:33 · Hardcoded primary key Booking No. 'E2EBOOK001' · microsoft/knowledge/testing/use-generateguid-for-unique-test-fixture-values.md
- major · medium · workshop/AmuseYou/Integration.Test/src/EndToEndBookingTests.Codeunit.al:75 · Hardcoded primary key Equipment No. 'CONCURRENT001' · microsoft/knowledge/testing/use-generateguid-for-unique-test-fixture-values.md
- major · medium · workshop/AmuseYou/Integration.Test/src/EndToEndBookingTests.Codeunit.al:79 · Hardcoded primary key Booking No. 'FIRST001' · microsoft/knowledge/testing/use-generateguid-for-unique-test-fixture-values.md
- major · medium · workshop/AmuseYou/Integration.Test/src/EndToEndBookingTests.Codeunit.al:89 · Hardcoded primary key Booking No. 'SECOND001'; the test asserts it does not exist · microsoft/knowledge/testing/use-generateguid-for-unique-test-fixture-values.md
- minor · medium · workshop/AmuseYou/RentalOperations/src/BookingMgt.Codeunit.al:65 · ProcessDailyBookings sets a constant Status per row with Modify; a single ModifyAll is equivalent · microsoft/knowledge/performance/prefer-modifyall-over-per-row-modify.md
- minor · medium · workshop/AmuseYou/RentalOperations/src/BookingMgt.Codeunit.al:65 · ProcessDailyBookings modifies iterated rows but uses FindSet() instead of FindSet(true) · microsoft/knowledge/performance/findset-true-applies-updlock-on-read.md
- minor · medium · workshop/AmuseYou/RentalOperations/src/RentalUpgrade.Codeunit.al:9 · Upgrade step has no HasUpgradeTag guard and never sets an upgrade tag · microsoft/knowledge/upgrade/use-upgrade-tags-not-version-checks.md
- minor · medium · workshop/AmuseYou/RentalOperations/src/RentalUpgrade.Codeunit.al:15 · New Equipment.Location field back-filled with row-based ModifyAll; DataTransfer is recommended · microsoft/knowledge/upgrade/datatransfer-for-bulk-init.md
- minor · high · workshop/AmuseYou/RentalOperations/src/BookingMgt.Codeunit.al:13 · Error() called with a hardcoded string literal instead of a Label · microsoft/knowledge/style/error-passes-parameters-directly-not-strsubstno.md
- minor · high · workshop/AmuseYou/RentalOperations/src/BookingMgt.Codeunit.al:16 · Error() with a hardcoded format string instead of a Label with parameters · microsoft/knowledge/style/error-passes-parameters-directly-not-strsubstno.md
- minor · medium · workshop/AmuseYou/RentalOperations/src/BookingList.Page.al:80 · Message() called with a hardcoded string literal instead of a Label · microsoft/knowledge/style/error-passes-parameters-directly-not-strsubstno.md
- minor · medium · workshop/AmuseYou/BookingCore.Test/src/BookingTests.Codeunit.al:5 · No [FEATURE] or [SCENARIO] tags · microsoft/knowledge/testing/test-feature-scenario-tags.md
- minor · medium · workshop/AmuseYou/RentalOperations.Test/src/UnitTests/AvailabilityTests.Codeunit.al:6 · No [FEATURE] or [SCENARIO] tags · microsoft/knowledge/testing/test-feature-scenario-tags.md
- minor · medium · workshop/AmuseYou/RentalOperations.Test/src/UnitTests/BookingMgtTests.Codeunit.al:6 · No [FEATURE] or [SCENARIO] tags · microsoft/knowledge/testing/test-feature-scenario-tags.md
- minor · medium · workshop/AmuseYou/Integration.Test/src/EndToEndBookingTests.Codeunit.al:6 · Test procedures carry [SCENARIO] but the codeunit has no [FEATURE] tag · microsoft/knowledge/testing/test-feature-scenario-tags.md
- minor · medium · workshop/AmuseYou/RentalOperations.Test/src/UnitTests/AvailabilityTests.Codeunit.al:61 · CreateTestBooking expects a Booking record but is passed Equipment1 (Record Equipment); will not compile · agent finding (agent:test-passes-wrong-record-type-to-helper)

Suppressed: microsoft/knowledge/performance/do-not-locktable-in-read-only-procedure.md (layer-precedence, displaced by custom/knowledge/performance/no-table-locks-in-user-actions.md)

## Verdicts

Not triaged yet.
