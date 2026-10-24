---
bc-version: [all]
domain: <style | testing | performance | security | upgrade>
keywords: [<3-10 lowercase kebab-case search terms>]
technologies: [al]
countries: [w1]
application-area: [all]
---

# <The rule, as one short statement>

## Description

<What the concern is and why it matters to our team. One or two short paragraphs.
Mention the card it came from: workshop/cards/<category>/<NN-name>.md, Side A or B.>

## Best Practice

<What we do. Be concrete enough that a reviewer can point at a line of code.>

## Anti Pattern

<What we don't do, and why. This section and Best Practice are the only parts
BCQuality treats as the rule. If it contradicts a Microsoft or Community rule,
ours wins in this repo, and the review lists the displaced rule under "suppressed".>

<!--
How to use this template (workshop "Write our rule" step):

1. Copy to custom/knowledge/<domain>/<slug>.md. <domain> must be one of the existing
   folders: style, testing, performance, security, upgrade. <slug> is kebab-case.
2. Keep all six frontmatter fields. A missing or empty field makes BCQuality skip
   the file silently.
3. No fenced code blocks. Put example code in sibling files:
   <slug>.good.al / <slug>.bad.al, and link them like [`<slug>.bad.al`](<slug>.bad.al).
4. Delete this comment.

Filled example (performance card 01, Side A):

---
bc-version: [all]
domain: performance
keywords: [locktable, user-action, page-action, concurrency, recalculate]
technologies: [al]
countries: [w1]
application-area: [all]
---

# No table-wide locks in user-triggered actions

## Description

A page action that locks a busy table (LockTable, or FindSet(true) over many rows)
blocks every other user for as long as the loop runs. From card
workshop/cards/performance/01-long-lock-user-action.md, Side A.

## Best Practice

Run bulk recalculations from a job queue entry or a background session, or lock
only the rows being changed, in small batches.

## Anti Pattern

Calling LockTable on Booking (or any table users write to all day) inside a
procedure that a page action triggers directly, then looping over open records.
-->
