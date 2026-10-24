---
bc-version: [all]
domain: performance
keywords: [locktable, user-action, page-action, concurrency, recalculate, blocking]
technologies: [al]
countries: [w1]
application-area: [all]
---

# No table-wide locks in user-triggered actions

## Description

A page action that locks a busy table (LockTable, or FindSet(true) over many rows) blocks every other user who writes to that table for as long as the loop runs. Users trigger page actions at any time of day, so the lock lands in the middle of other people's work. From card workshop/cards/performance/01-long-lock-user-action.md, Side A.

## Best Practice

Run bulk recalculations triggered from a page from a job queue entry or a background session, or change only the affected rows in small batches without a table-wide lock.

## Anti Pattern

Calling LockTable on a table users write to all day (such as Booking) in a procedure that a page action calls directly, and then looping over and modifying the open records while the lock is held.
