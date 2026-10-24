---
category: code-quality
bcquality_domain: style
tag: bites
seed: workshop/AmuseYou/RentalOperations/src/BookingMgt.Codeunit.al
---

# Should a management codeunit ever open UI (Page.Run, Message, Confirm)?

**Side A:** No. Business logic codeunits must stay UI-free so they can run from APIs, job queues and tests. UI belongs on the page.

**Side B:** A thin "open the card for this number" helper is fine. Making every caller repeat a Get + Page.Run is worse than one well-named UI procedure.
