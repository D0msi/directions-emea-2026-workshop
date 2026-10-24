# Facilitator guide: running this lab with your own team

A minimal version for Paris. It covers the shape of the session; your own codebase and cards make it real.

## What you need

- A copy of BCQuality your team controls (see [`SHARING-PATTERNS.md`](SHARING-PATTERNS.md), pattern A), with this `workshop/` folder and `AGENTS.md` added.
- Some code to review. Small is better: 5-10 files that every pair reviews, listed in `AGENTS.md` under **Review scope**.
- A set of stance cards (`workshop/cards/`). Each is a question with **two defensible sides**. A few should point at a concrete pattern in the review scope (🦷 "bites"), so the rule a pair writes visibly fires on the rerun.
- The gate workflow (`.github/workflows/review-receipt-gate.yml`). Set the repo's "require approval for first-time contributors" option to the least strict setting, or every first PR waits for a click.

## Session shape (105 min)

| Time | Block | Notes |
|---|---|---|
| 0-10 | Clone, open, "Read AGENTS.md and tell me what this workshop is" | Pair people up. Mixed agents per pair are a feature. |
| 10-30 | Lab 1: "Run the BCQuality Review", compare with your partner | A review takes about 5-10 min per agent. Ask: same findings? same severity? who's right? |
| 30-55 | Lab 2: pick a card, pick a side, "Write our rule" | The rerun should show the rule firing (or displacing a Microsoft rule). |
| 55-75 | Lab 3: "Submit our rule" | One PR per pair. Watch the gate go green/red on the projector. |
| 75-90 | Merge moment: merge a few PRs live, everyone "Get the room's latest" | New findings appear without anyone changing the code. Discuss any contradictions. |
| 90-105 | Take-home: "Pipeline lab step", wrap-up | Nothing is run or committed. |

## Things that go wrong

- **An agent can't run the review.** Point at `workshop/fallbacks/` and pair them with a working agent.
- **Two agents disagree on severity.** Good. That's the conversation the lab is for: agree on a rule, write it down.
- **The gate is red.** Read the message in the check. Usually: no report committed, an open blocker without a verdict, or a file outside `custom/knowledge/` and `workshop/reviews/`.
- **Contradicting rules after the merge.** Don't resolve them automatically. Decide as a team, keep one, and record it in `TEAM-DECISIONS.md`.

## Keep for yourself, not in the repo

Seed lists, planted rules and answer notes should live outside the attendee repo (this repo gitignores `workshop/.instructor/`). Attendees' agents read everything in the clone.
