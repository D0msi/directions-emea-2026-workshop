# Fallbacks

Prepared outputs for anyone whose agent isn't working. Read them, compare with your partner, and carry on with the next lab.

## `lab1-done/`: the same review, two different agents

Both reports are real runs of **"Run the BCQuality Review"** on the same 10 files, same BCQuality commit, from a fresh clone, following only `AGENTS.md`.

| Report | Reviewed with | Blockers | Major | Minor |
|---|---|---|---|---|
| [`fallback-opus.md`](lab1-done/fallback-opus.md) | Claude Code (Claude Opus 5.5) | 1 | 11 | 14 |
| [`fallback-sonnet.md`](lab1-done/fallback-sonnet.md) | Claude Code (Claude Sonnet 5.5) | 0 | 12 | 14 |

Things to look for:
- **The same finding, different severity.** The `asserterror` test in `EndToEndBookingTests.Codeunit.al` is a **blocker** for one and **major** for the other. One PR gate goes red, the other green.
- **Rules only one agent applied**, e.g. `validate-unauthenticated-response-before-use` and `datatransfer-for-bulk-init` (Sonnet only), `tooltip-required-on-page-fields` (Opus only).
- **Agent findings** (no rule behind them, capped at minor): which ones would you turn into a team rule?

## `lab2-done/`

A finished rule from a stance card and a rerun report showing it fire. *(Coming.)*
