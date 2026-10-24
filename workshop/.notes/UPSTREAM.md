# Upstream source

This room repo is a **copy** (not a GitHub fork) of BCQuality, so attendee PRs can never reach the real repo.

| | |
|---|---|
| Source repo | https://github.com/microsoft/BCQuality |
| Branch | `main` |
| Commit | `a4593a1b1585a0fa663efbf39ff02c60f846d851` |
| Commit date | 2026-10-20T17:59:45+02:00 |
| Commit subject | Route guard-dependent and/or conditions to the short-circuit article (#249) |
| Plugin version at that commit | 1.9.0 (latest tag `v1.9`) |
| Copied on | 2026-10-24 |

## Changes made to BCQuality content in this copy

- Removed all upstream workflows in `.github/workflows/` (`flag-new-top-level`, `guard-custom-layer`, `knowledge-index`, `release-version`, `review-fixtures`, `skill-index`, `validate-frontmatter`) so only the workshop gate runs on attendee PRs.
- Removed `.github/dependabot.yml` (would open action-bump PRs against the room repo).
- Everything else outside `/custom/` is untouched. `LICENSE` (MIT) and attribution are intact.

## Syncing later

There is no upstream link in GitHub. To sync manually:

```bash
git remote add upstream https://github.com/microsoft/BCQuality.git
git fetch upstream main
git merge upstream/main   # re-delete any workflows upstream re-adds
```
