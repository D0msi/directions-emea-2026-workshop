# Sharing patterns: how a team keeps one set of rules

Two ways to give every developer (and every agent) the same BCQuality rules plus your own.

## Pattern A: your own copy of BCQuality, rules in `/custom/`

Your organization owns a repository that **is** BCQuality, and your rules live in its `custom/knowledge/` folder. This is the layout BCQuality documents (`custom/README.md`, `docs/customizing-bcquality.md`), and it's what this room repo does.

Two variants:

| | GitHub fork | Copy (duplicate) |
|---|---|---|
| How | Fork button | Clone BCQuality, push to a new empty repo |
| Visibility | **Same as upstream: a fork of a public repo can't be private** | Anything you like, including private |
| Upstream sync | "Sync fork" button, or merge `upstream/main` | Manual: add an `upstream` remote, fetch, merge |
| PR risk | PRs default to the upstream repo, so someone *will* open your rules against `microsoft/BCQuality` eventually | None: there's no fork link |

Because a fork of a public repo can't be private, most organizations **copy** instead. This room repo is a copy for the same reason as the PR-risk row: 30 attendee PRs must never land on the real BCQuality. The cost is the manual sync (see `workshop/.notes/UPSTREAM.md`).

**Good for:** one team, one set of rules, agents pointed at one folder.
**Watch out for:** every upstream sync is a merge into a repo you've changed. Keep your changes inside `/custom/` and it stays a clean merge.

## Pattern B: an org repo that contains BCQuality

Your organization's "engineering standards" repo holds your own skills, agents and rules, and pulls BCQuality in as a dependency: a git submodule, a pinned download in CI, or a plugin install at a pinned version.

**Good for:** organizations that have more than quality rules to share (agent definitions, prompts, coding skills, pipeline templates) and want BCQuality as one input among several.

Things to plan for:
- **Pinned version, deliberate upgrades.** Pin BCQuality to a tag or commit (BCQuality tags releases, e.g. `v1.9`). Upgrading is a PR in your repo that bumps the pin, with a rerun of representative reviews before merging. BCQuality's own guidance: record `git rev-parse HEAD` for reproducible runs (`docs/customizing-bcquality.md`, "Updates and versions").
- **Submodule friction.** Submodules need `git clone --recurse-submodules` (or `git submodule update --init`), detach `HEAD`, and confuse people who just want to edit a rule. Shallow clones of the parent don't shallow the submodule unless asked. A pinned download step in CI is often simpler than a submodule.
- **Where your custom rules go.** BCQuality's review only discovers rules under `<layer>/knowledge/` inside the BCQuality root. In pattern B you either place your rules into the embedded copy's `custom/knowledge/` at build time, or keep a pattern A copy as the dependency.

## Where non-quality org content lives

BCQuality is quality knowledge and review skills. Your other shared agent material doesn't belong in its `/custom/`:

| Content | Where |
|---|---|
| Team review rules (AL quality) | `custom/knowledge/<domain>/` in your BCQuality copy (A), or the embedded one (B) |
| Org-specific review skills | `custom/skills/` (they follow BCQuality's DO contract) |
| Agent instructions for app repos (`AGENTS.md`), coding skills, prompts, agent definitions | Your org standards repo (pattern B), or a template that app repos copy |
| Pipeline templates (the review-receipt gate) | Your org standards / pipeline template repo |

## Which one?

Start with **A (copy)**. It's one repo, one folder, and exactly what BCQuality documents. Move to **B** when you have enough non-quality agent content that it needs a home of its own, and keep BCQuality pinned inside it.
