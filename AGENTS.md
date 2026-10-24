# AGENTS.md: Directions EMEA 2026 workshop

Instructions for any AI agent (Claude Code, GitHub Copilot, Cursor, Codex, ...) working in this repo during the hands-on lab **"Agentic as a Team Sport: Shared Knowledge, Standards, and Pipelines"**.

This repo is a workshop copy of [BCQuality](https://github.com/microsoft/BCQuality) (MIT) with the lab material added under `workshop/`. Attendees work in pairs, and each person drives their own agent.

`CLAUDE.md` and `.github/copilot-instructions.md` only point here. This file is the single source of instructions.

## How to answer in this workshop

- **Keep answers short.** People read them on a projector or a laptop shared with a neighbour.
- **Always show file paths** (repo-relative) for anything you read or write.
- When the user types one of the step names below (or something close to it), run that step exactly as written. If unsure which step they mean, list the step names and ask.
- Ask at most one question at a time.

## Where things live

| Path | What |
|---|---|
| `skills/entry.md`, `skills/read.md`, `skills/do.md` | BCQuality's review instructions (router, knowledge contract, action-skill contract). Treat them as authoritative. |
| `microsoft/knowledge/`, `community/knowledge/` | BCQuality's rules. **Read only.** |
| `custom/knowledge/<domain>/` | **The room's rules.** Pairs add one rule each here. |
| `custom/_TEMPLATE.md` | Template for a new rule. |
| `workshop/AmuseYou/` | The AL code under review (Business Central apps + test apps). |
| `workshop/cards/<category>/` | Stance cards: a question with two defensible sides. |
| `workshop/reviews/` | Review reports (one per pair). |
| `workshop/TEAM-DECISIONS.md` | Plain record of decisions. Not an input to the review. |
| `workshop/fallbacks/` | Prepared outputs for anyone without a working agent. |
| `workshop/pipelines/` | The PR gate script and reference pipelines. |

## Guardrails (all steps)

- **Never edit anything under `workshop/AmuseYou/`.** We review it, we don't fix it.
- **Never edit** `microsoft/`, `community/`, `skills/`, `tools/`, `schemas/` or `docs/`.
- **Never push to `main`** of the room repo, and never open anything against `microsoft/BCQuality`.
- Nothing in this lab compiles AL, downloads symbols or needs a Business Central sandbox. Don't try.
- Don't run `git` commands that discard work (`reset --hard`, `clean`, `checkout -- .`) unless the user explicitly asks.

## Constants

- **Room repo:** `https://github.com/JeremyVyska/directions-emea-2026-workshop.git` (owner/name: `JeremyVyska/directions-emea-2026-workshop`)
- **Review scope** (the files every review covers):
  1. `workshop/AmuseYou/RentalOperations/src/BookingMgt.Codeunit.al`
  2. `workshop/AmuseYou/RentalOperations/src/AvailabilityMgt.Codeunit.al`
  3. `workshop/AmuseYou/RentalOperations/src/WeatherApiClient.Codeunit.al`
  4. `workshop/AmuseYou/RentalOperations/src/BookingList.Page.al`
  5. `workshop/AmuseYou/RentalOperations/src/RentalUpgrade.Codeunit.al`
  6. `workshop/AmuseYou/BookingCore/src/BookingStatus.Enum.al`
  7. `workshop/AmuseYou/BookingCore.Test/src/BookingTests.Codeunit.al`
  8. `workshop/AmuseYou/RentalOperations.Test/src/UnitTests/AvailabilityTests.Codeunit.al`
  9. `workshop/AmuseYou/RentalOperations.Test/src/UnitTests/BookingMgtTests.Codeunit.al`
  10. `workshop/AmuseYou/Integration.Test/src/EndToEndBookingTests.Codeunit.al`
- **Review goal** (copy verbatim into `goal`): `Review style, testing, performance, security and upgrade concerns in these AL files without changing them.`

---

## Step: "Read AGENTS.md and tell me what this workshop is"

Read-only. Reply in at most 6 lines:
1. One sentence: what the workshop is (pairs review the same code with different agents, agree on team rules, share them through a PR).
2. Confirm you can see `skills/entry.md`, `custom/knowledge/`, `workshop/TEAM-DECISIONS.md` and `workshop/AmuseYou/`.
3. Say which agent/model you are.
4. List the step names below.

## Step: "Run the BCQuality Review"

Do not change any file except the report.

1. If you don't know the pair's slug yet, ask once: "What's your pair name? (lowercase, dashes, e.g. `paris-07`)". The report goes to `workshop/reviews/<pair-slug>.md`.
2. Prepare the knowledge index, if PowerShell 7 is available: run `pwsh ./tools/Build-KnowledgeIndex.ps1` from the repo root. If `pwsh` is missing or this fails, carry on: BCQuality falls back to reading the folders.
3. Read `skills/entry.md` and follow it, with the repo root as the BCQuality root and this task context:
   ```yaml
   task-context:
     goal: Review style, testing, performance, security and upgrade concerns in these AL files without changing them.
     inputs-available: [file-path]
     technologies: [al]
     bc-version: 25
     countries: [w1]
     application-area: [all]
     enabled-layers: [microsoft, community, custom]
     disabled-skills:
       - microsoft/skills/review/al-code-review.md
       - microsoft/skills/review/al-appsource-review.md
       - microsoft/skills/review/al-breaking-changes-review.md
       - microsoft/skills/review/al-data-modeling-review.md
       - microsoft/skills/review/al-error-handling-review.md
       - microsoft/skills/review/al-events-review.md
       - microsoft/skills/review/al-finance-review.md
       - microsoft/skills/review/al-interfaces-review.md
       - microsoft/skills/review/al-privacy-review.md
       - microsoft/skills/review/al-query-review.md
       - microsoft/skills/review/al-reporting-review.md
       - microsoft/skills/review/al-scm-review.md
       - microsoft/skills/review/al-telemetry-review.md
       - microsoft/skills/review/al-ui-review.md
       - microsoft/skills/review/al-web-services-review.md
       - microsoft/skills/development/al-development-plan.md
       - community/skills/review/al-agents-review.md
       - community/skills/review/al-copilot-review.md
   ```
   - The `file-path` input is every file in **Review scope** above.
   - `bc-version: 25` comes from the apps' `app.json` (`"application": "25.0.0.0"`). That isn't a guess, so use it. `countries` and `application-area` are known too, so every dimension is supplied.
   - Entry should dispatch exactly five skills: `al-style-review`, `al-testing-review`, `al-performance-review`, `al-security-review` and `al-upgrade-review`. Everything else is disabled on purpose, so every pair in the room runs the same review.
4. Execute every dispatched action skill as Entry and `skills/do.md` describe, reading `skills/read.md` on demand. In this lab:
   - Only leaf skills run, so the **Composition (super-skills)** parts of `skills/do.md` don't apply.
   - Rules in `custom/knowledge/` are part of the review. Where one contradicts a Microsoft or Community rule, the custom rule wins, and the losing rule goes in the report's suppressed list.
   - **Two custom rules that contradict each other** on the same code: precedence can't decide, so don't pick a winner. Report one `info` finding at that location citing both rule paths, with the message starting `Disputed:`. The room settles it.
   - Include conditionally applicable rules (READ caps their findings at `medium` confidence).
   - You may **read** files outside the review scope for context (e.g. the tables a codeunit uses), but report findings only on scope files.
   - Keep each skill's JSON findings report in memory or in a temp folder **outside the repo**. Running `tools/Validate-FindingsReport.ps1` is optional.
   - BCQuality's PowerShell helpers take arrays. Call them from a `pwsh` session or `pwsh -Command "& ./tools/... -Paths @('a','b')"`, not with `pwsh -File`, which can't pass arrays.
5. Write the report in the **Review report contract** format below. Copy findings, severities and rule paths from the skills' findings reports; don't invent findings, rule names or counts. Entry's dispatch record doesn't need to be shown or saved.
6. Reply with: the report path, the counts per severity, and the three findings you'd discuss first with your partner. Nothing more.

If the review can't finish (time, context limits), write the report anyway with `outcome: partial` and say what was skipped.

Expect this step to take **5–10 minutes**. Running the five skills in parallel (subagents or child contexts, if your agent has them) makes it faster.

## Step: "Write our rule"

One rule file per pair.

1. Ask: "Which card? (path or category + number, e.g. `performance 01`)". Read the card from `workshop/cards/`.
2. Ask: "Side A, Side B, or your own wording?" Then ask for **one sentence of why**.
   Advanced pairs may skip the card and give a free-form rule. Then ask which domain it belongs to: `style`, `testing`, `performance`, `security` or `upgrade`.
3. Create `custom/knowledge/<domain>/<rule-slug>.md` from `custom/_TEMPLATE.md`. Use the card's `bcquality_domain` as `<domain>` and a short kebab-case `<rule-slug>` that describes the rule. Requirements (BCQuality's `skills/read.md`):
   - All six frontmatter fields present and non-empty: `bc-version`, `domain`, `keywords`, `technologies`, `countries`, `application-area`.
   - `# Title`, `## Description` (mention the card path and side), `## Best Practice`, `## Anti Pattern`. The stance must be in Best Practice / Anti Pattern, or the review ignores it.
   - **No fenced code blocks.** Code goes in a sibling `<rule-slug>.bad.al` / `<rule-slug>.good.al` file, linked from the text.
   - Delete the template's instruction comment.
4. Show the file path and the rule's title, Best Practice and Anti Pattern (max 8 lines). Ask "OK?" and adjust if needed.
5. Rerun **"Run the BCQuality Review"** with the pair's slug (ask once if you don't know it). It overwrites `workshop/reviews/<pair-slug>.md` in the **Review report contract** format. **This report is the pair's receipt for Lab 3**, so "Submit our rule" commits it as-is, with no new review. Keep the `## Verdicts` entries from the earlier report whose findings still appear, and recompute `blockers`.
6. Say in one or two lines **whether the new rule fired**: a finding citing `custom/knowledge/...`, or a Microsoft rule now listed as suppressed. If it didn't fire, say why you think so (e.g. the scope has no code the rule applies to).
7. If `blockers` is above 0, say: "The Lab 3 gate will be red until each blocker has an accept/reject verdict. Want to record verdicts now?"

## Step: "Submit our rule"

The PR may contain **only** the pair's rule (plus optional sibling `.al` samples) under `custom/knowledge/` and the pair's report under `workshop/reviews/`. One person per pair does this.

**Don't run a new review in this step.** The receipt is the report the "Write our rule" rerun already wrote.

1. Check `git status`. Identify the rule file(s) under `custom/knowledge/` and the report `workshop/reviews/<pair-slug>.md`. If anything else is modified, list it and say it will be left out of the PR.
2. Reuse that report as the receipt. Only if it's **missing**, or the rule's Best Practice / Anti Pattern text was changed after the report was written, say so and ask: "Rerun the review now (~5 min), or submit as-is?"
3. If the report shows open blockers (`blockers` above 0), say so: the gate will go red. Offer to record verdicts: add `accept: <reason>` or `reject: <reason>` lines under `## Verdicts` and recompute `blockers`. That's an edit to the report, not a new review. Don't edit AmuseYou code.
4. Create a branch `rule/<rule-slug>` (the rule file's name without `.md`) and commit **only** those paths, with message `Rule: <rule title>`.
   Then run the gate locally, exactly as the PR check will: `pwsh ./workshop/pipelines/gate/check-review-receipt.ps1 -BaseRef origin/main -RestrictPaths`. If `pwsh` isn't installed, skip this; the PR check runs it anyway. If it fails, show the messages and fix the report or rule before pushing.
5. Fork and push:
   - **If `gh` is installed and logged in** (`gh auth status`):
     1. `gh repo fork JeremyVyska/directions-emea-2026-workshop --clone=false`
     2. `git remote add fork https://github.com/<their-login>/directions-emea-2026-workshop.git` (get the login with `gh api user --jq .login`; skip if the remote exists).
     3. `git push fork rule/<rule-slug>`
     4. `gh pr create --repo JeremyVyska/directions-emea-2026-workshop --base main --head <their-login>:rule/<rule-slug> --title "Rule: <rule title>"`, with a body following `.github/pull_request_template.md`.
   - **Without `gh`:** walk the user through it one step at a time: open https://github.com/JeremyVyska/directions-emea-2026-workshop, click **Fork**, give you the fork URL, and you run the `git remote add fork ...` and `git push` above. Then give them the compare link `https://github.com/JeremyVyska/directions-emea-2026-workshop/compare/main...<their-login>:directions-emea-2026-workshop:rule/<rule-slug>` to open the PR.
   - The PR's **base must be `JeremyVyska/directions-emea-2026-workshop` `main`**. Never `microsoft/BCQuality`.
6. Reply with the PR link and: "The **Review receipt** check will go green or red in about a minute."

## Step: "Get the room's latest"

1. If there are uncommitted changes, list them and ask whether to commit or stash them first. Don't discard anything.
2. Switch to `main` and pull the room's `main` **by URL**, not from whatever remote happens to be called `origin`:
   ```text
   git switch main
   git pull --ff-only https://github.com/JeremyVyska/directions-emea-2026-workshop.git main
   ```
   If fast-forward fails, stop and show the error. Don't force anything.
3. List the rule files that are new in `custom/knowledge/` (path + title).
4. Rerun **"Run the BCQuality Review"** (same slug) and report findings that are **new compared with the previous report**, especially ones citing `custom/knowledge/...`.
5. **Contradiction check:** read every file in `custom/knowledge/`. List pairs of rules whose Best Practice / Anti Pattern contradict each other with overlapping applicability (same code could be judged both ways). For each pair: both paths and one line on the conflict. **Do not resolve, edit or delete anything.** Contradictions are for the room to discuss. If there are none, say so.

## Step: "Pipeline lab step"

Take-home. **Don't run, don't commit, don't push.**

1. Ask: "GitHub Actions or Azure DevOps?"
2. Copy the matching reference file into `workshop/pipelines/generated/` (gitignored):
   - GitHub Actions: `workshop/pipelines/reference/github.yml` → `workshop/pipelines/generated/review-receipt-gate.yml`
   - Azure DevOps: `workshop/pipelines/reference/ado.yml` → `workshop/pipelines/generated/azure-pipelines.yml`
3. Ask, one at a time:
   - "What's your default branch?" (default `main`)
   - "Where should review reports live in your app repo?" (default `reviews`)
   - "Where will your team's BCQuality copy be checked out on a developer machine?" (e.g. `C:\Repos\CompanyBCQuality`; see `workshop/SHARING-PATTERNS.md`)

   Adapt the branch and `REPORTS` in the yml copy. Keep the gate script call unchanged apart from paths. **Don't add `-RestrictPaths`**: that's only for a rules repo like this one.
4. Copy `workshop/pipelines/reference/AGENTS-review-section.md` to `workshop/pipelines/generated/AGENTS-review-section.md` and fill in its placeholders (`<BCQUALITY_ROOT>`, `<REPORTS>`, `<BASE>`) from the answers. This is the "Run the BCQuality Review" step for **their** repo: it reviews a branch's diff instead of this room's fixed file list. They paste it into their app repo's `AGENTS.md`.
5. List the plug-in steps they'll do back home (from the comments in the reference files):
   - where the yml goes in their repo,
   - copying `workshop/pipelines/gate/check-review-receipt.ps1` into their repo,
   - pasting the generated section into their repo's `AGENTS.md`,
   - making the check required (GitHub: branch protection / ruleset required status check; Azure DevOps: build validation branch policy).
6. Point them at the worked example: https://github.com/JeremyVyska/directions-emea-2026-workshop-demo (an AL app repo with the gate set up; PR #1 has no receipt and is red, PR #2 has one and is green).
7. Remind them: no LLM, API key or secret goes in the pipeline. The review stays local.

## No agent?

Everything has a prepared output in `workshop/fallbacks/`:
- `workshop/fallbacks/lab1-done/`: two review reports of the same code by two different agents. Compare them with your partner.
- `workshop/fallbacks/lab2-done/`: a finished rule and a rerun report showing it fire.
For Lab 3 you can submit through the browser: on GitHub, **Add file → Create new file** in `custom/knowledge/<domain>/`. GitHub creates the fork and the PR for you. The gate will go red without a report, which is the point.

---

## Review report contract

Path: `workshop/reviews/<pair-slug>.md`. The PR gate (`workshop/pipelines/gate/check-review-receipt.ps1`) parses this, so keep the shape exact.

```markdown
---
reviewed_with: <agent and model, e.g. "GitHub Copilot (agent mode, GPT-x)">
reviewed_files: [<repo-relative paths reviewed>]
bcquality_commit: <output of `git rev-parse --short HEAD`>
outcome: <completed | partial | failed>   # the findings report's outcome; worst one if several
blockers_found: <integer>                 # blocker-severity findings in the review
blockers: <integer>                       # OPEN blockers: blockers_found minus those with an accept/reject verdict
---

# BCQuality Review: <pair-slug>

## Blockers

<One bullet per blocker-severity finding: `id` · file:line · one-line message · cited rule path. "None." if none.>

## Findings

<One bullet per major/minor/info finding, most severe first: severity · confidence · file:line · message · cited rule path (or "agent finding").
Then a line "Suppressed:" listing any rule displaced by layer precedence.>

## Verdicts

<One bullet per finding the pair has discussed: `<finding id or file:line>` → `fix` | `accept: <reason>` | `reject: <reason>`.
Empty (write "Not triaged yet.") straight after a review.>
```

Rules:
- Nothing between the `# BCQuality Review` title and `## Blockers` except, optionally, one short summary line.
- **One bullet per rule per file.** When a rule fires on several lines of one file, write one bullet listing the lines (`file:12, 40, 61`). When it fires in three files, write three bullets. Severity counts are bullet counts.
- Copy each finding's severity from the skill that produced it. Don't adjust severities afterwards. A severity that changes because a different rule now applies is expected.
- With nothing suppressed, write `Suppressed: None.`
- `reviewed_with`: the agent product, plus the model in brackets if known, e.g. `GitHub Copilot (agent mode, GPT-x)` or `Claude Code (Claude Sonnet x)`.
- `blockers` counts blockers that have **no** `accept` or `reject` verdict. A `fix` verdict leaves the blocker open, since AmuseYou isn't changed in this lab.
- When the review is rerun, keep the existing `## Verdicts` entries whose findings still appear, and recompute `blockers`.
