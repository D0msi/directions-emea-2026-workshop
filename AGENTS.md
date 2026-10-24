# AGENTS.md: Directions EMEA 2026 workshop

Instructions for any AI agent (Claude Code, GitHub Copilot, Cursor, Codex, ...) working in this repo during the hands-on lab **"Agentic as a Team Sport: Shared Knowledge, Standards, and Pipelines"**.

This repo is a workshop copy of [BCQuality](https://github.com/microsoft/BCQuality) (MIT) with the lab material added under `workshop/`. Attendees work in pairs, and each person drives their own agent.

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

1. If you don't know the pair's slug yet, ask once: "What's your pair name? (lowercase, dashes, e.g. `paris-07`)". The report goes to `workshop/reviews/<slug>.md`.
2. Prepare the knowledge index, if PowerShell 7 is available: run `pwsh ./tools/Build-KnowledgeIndex.ps1` from the repo root. If `pwsh` is missing or this fails, carry on: BCQuality falls back to reading the folders.
3. Read `skills/entry.md` and follow it, with the repo root as the BCQuality root and this task context:
   ```yaml
   task-context:
     goal: Review style, testing, performance, security and upgrade concerns in these AL files without changing them.
     inputs-available: [file-path]
     technologies: [al]
     enabled-layers: [microsoft, community, custom]
     disabled-skills: []
   ```
   The `file-path` input is every file in **Review scope** above. Do not guess `bc-version`.
4. Execute every dispatched action skill as Entry and `skills/do.md` describe, reading `skills/read.md` on demand. Rules in `custom/knowledge/` are part of the review. Where one contradicts a Microsoft or Community rule, the custom rule wins, and the losing rule goes in the report's suppressed list.
5. Write the report in the **Review report contract** format below. Base it on the findings reports; don't invent findings, rule names or counts.
6. Reply with: the report path, the counts per severity, and the three findings you'd discuss first with your partner. Nothing more.

If the review can't finish (time, context limits), write the report anyway with `outcome: partial` and say what was skipped.

## Step: "Write our rule"

One rule file per pair.

1. Ask: "Which card? (path or category + number, e.g. `performance 01`)". Read the card from `workshop/cards/`.
2. Ask: "Side A, Side B, or your own wording?" Then ask for **one sentence of why**.
   Advanced pairs may skip the card and give a free-form rule. Then ask which domain it belongs to: `style`, `testing`, `performance`, `security` or `upgrade`.
3. Create `custom/knowledge/<domain>/<slug>.md` from `custom/_TEMPLATE.md`. Use the card's `bcquality_domain` as `<domain>` and a short kebab-case `<slug>` that describes the rule. Requirements (BCQuality's `skills/read.md`):
   - All six frontmatter fields present and non-empty: `bc-version`, `domain`, `keywords`, `technologies`, `countries`, `application-area`.
   - `# Title`, `## Description` (mention the card path and side), `## Best Practice`, `## Anti Pattern`. The stance must be in Best Practice / Anti Pattern, or the review ignores it.
   - **No fenced code blocks.** Code goes in a sibling `<slug>.bad.al` / `<slug>.good.al` file, linked from the text.
   - Delete the template's instruction comment.
4. Show the file path and the rule's title, Best Practice and Anti Pattern (max 8 lines). Ask "OK?" and adjust if needed.
5. Rerun **"Run the BCQuality Review"** (same slug: overwrite the report). Then say in one or two lines **whether the new rule fired**: a finding citing `custom/knowledge/...`, or a Microsoft rule now listed as suppressed. If it didn't fire, say why you think so (e.g. the scope has no code the rule applies to).

## Step: "Submit our rule"

The PR may contain **only** the pair's rule (plus optional sibling `.al` samples) under `custom/knowledge/` and the pair's report under `workshop/reviews/`. One person per pair does this.

1. Check `git status`. Identify the rule file(s) and the report. If anything else is modified, list it and say it will be left out of the PR.
2. If the rule changed after the report was written, rerun **"Run the BCQuality Review"** first.
3. If the report shows open blockers (`blockers` above 0), say so: the gate will go red. Offer to record verdicts (see the contract) and rerun, but don't edit AmuseYou code.
4. Create a branch `rule/<slug>` and commit **only** those paths, with message `Rule: <rule title>`.
5. Fork and push:
   - **If `gh` is installed and logged in** (`gh auth status`):
     1. `gh repo fork JeremyVyska/directions-emea-2026-workshop --clone=false`
     2. `git remote add fork https://github.com/<their-login>/directions-emea-2026-workshop.git` (get the login with `gh api user --jq .login`; skip if the remote exists).
     3. `git push fork rule/<slug>`
     4. `gh pr create --repo JeremyVyska/directions-emea-2026-workshop --base main --head <their-login>:rule/<slug> --title "Rule: <rule title>"`, with a body following `.github/pull_request_template.md`.
   - **Without `gh`:** walk the user through it one step at a time: open https://github.com/JeremyVyska/directions-emea-2026-workshop, click **Fork**, give you the fork URL, and you run the `git remote add fork ...` and `git push` above. Then give them the compare link `https://github.com/JeremyVyska/directions-emea-2026-workshop/compare/main...<their-login>:directions-emea-2026-workshop:rule/<slug>` to open the PR.
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
3. Ask what their repo looks like (where reviews and rules should live; default branch name) and adapt the paths and branch in the copy. Keep the gate script call unchanged apart from paths.
4. List the plug-in steps they'll do back home (from the comments in the reference file):
   - where the yml goes in their repo,
   - copying `workshop/pipelines/gate/check-review-receipt.ps1` into their repo,
   - making the check required (GitHub: branch protection / ruleset required status check; Azure DevOps: build validation branch policy).
5. Remind them: no LLM, API key or secret goes in the pipeline. The review stays local.

## No agent?

Everything has a prepared output in `workshop/fallbacks/`:
- `workshop/fallbacks/lab1-done/`: two review reports of the same code by two different agents. Compare them with your partner.
- `workshop/fallbacks/lab2-done/`: a finished rule and a rerun report showing it fire.
For Lab 3 you can submit through the browser: on GitHub, **Add file → Create new file** in `custom/knowledge/<domain>/`. GitHub creates the fork and the PR for you. The gate will go red without a report, which is the point.

---

## Review report contract

Path: `workshop/reviews/<slug>.md`. The PR gate (`workshop/pipelines/gate/check-review-receipt.ps1`) parses this, so keep the shape exact.

```markdown
---
reviewed_with: <agent and model, e.g. "GitHub Copilot (agent mode, GPT-x)">
reviewed_files: [<repo-relative paths reviewed>]
bcquality_commit: <output of `git rev-parse --short HEAD`>
outcome: <completed | partial | failed>   # the findings report's outcome; worst one if several
blockers_found: <integer>                 # blocker-severity findings in the review
blockers: <integer>                       # OPEN blockers: blockers_found minus those with an accept/reject verdict
---

# BCQuality Review: <slug>

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
- `blockers` counts blockers that have **no** `accept` or `reject` verdict. A `fix` verdict leaves the blocker open, since AmuseYou isn't changed in this lab.
- Copy severities and rule paths from the findings reports. Never raise or lower a severity in the report.
- When the review is rerun, keep the existing `## Verdicts` entries whose findings still appear, and recompute `blockers`.
