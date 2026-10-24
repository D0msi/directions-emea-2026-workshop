# Agentic as a Team Sport: hands-on lab

**Directions EMEA 2026, Paris** · Shared Knowledge, Standards, and Pipelines

> This is a workshop copy of [microsoft/BCQuality](https://github.com/microsoft/BCQuality) (MIT), with the lab material added under `workshop/`. For BCQuality itself, use the real repo.

## Quick start (2 minutes)

1. Clone (shallow is enough):
   ```bash
   git clone --depth 1 https://github.com/JeremyVyska/directions-emea-2026-workshop.git
   ```
2. Open the folder in VS Code.
3. Start your AI agent in that folder (Claude Code, GitHub Copilot agent mode, Cursor, Codex, anything that can read files).
4. Type:
   > Read AGENTS.md and tell me what this workshop is
5. Find a partner. You'll work in **pairs**, each with your own agent.

No AL compiler, symbols or Business Central sandbox needed. PowerShell 7 (`pwsh`) makes the review faster but is optional.

## The labs

| Lab | You type | What happens |
|---|---|---|
| 1 | **"Run the BCQuality Review"** | Your agent reviews the AmuseYou code with BCQuality and writes a report to `workshop/reviews/`. Compare it with your partner's. |
| 2 | **"Write our rule"** | Pick a stance card from `workshop/cards/`, pick a side, and your agent writes a team rule into `custom/knowledge/`. Rerun the review and watch the rule fire. |
| 3 | **"Submit our rule"** | One person per pair opens a PR to the room repo. A gate checks your review report. |
| Merge | **"Get the room's latest"** | After rules are merged live, pull them, rerun the review, and look for rules that contradict each other. |
| Take-home | **"Pipeline lab step"** | Generate a GitHub Actions or Azure DevOps gate for your own repo. Nothing is run or committed. |

The exact steps your agent follows are in [`AGENTS.md`](../AGENTS.md).

## No agent, or it's misbehaving?

Use the prepared outputs in [`workshop/fallbacks/`](fallbacks/), and pair up with someone whose agent works. For Lab 3 you can add a file through the GitHub web UI (**Add file → Create new file** under `custom/knowledge/<domain>/`). GitHub makes the fork and the PR for you.
