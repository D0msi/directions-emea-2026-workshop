# BCQuality conventions (as inspected)

Facilitator notes. Source: `microsoft/BCQuality` @ `a4593a1` (see `UPSTREAM.md`). Everything here is read from the repo itself; file references are given so it can be re-checked.

## 1. How a review is invoked

BCQuality is content, not a program. "Invoke `skills/entry.md`" means *ask the agent to read and follow that file* (`docs/agent-consumption.md`, "Try a minimal integration").

Flow: **Entry** (`skills/entry.md`) builds `knowledge-index.json`, routes the task to one or more **action skills** under `*/skills/**`, the agent runs them using the **READ** (`skills/read.md`) and **DO** (`skills/do.md`) contracts, and each returns a **findings-report** (`schemas/findings-report.schema.json`).

### Claude Code

Two routes:

1. **Direct reading (recommended for the room).** The cloned room repo *is* a BCQuality checkout, so the BCQuality root is the repo root. The agent reads `skills/entry.md` with a task context. Same as any other agent (below).
2. **As a plugin.** The repo ships `.claude-plugin/marketplace.json`, so `/plugin marketplace add <path>` + install would expose `al-code-review`. Not worth it in the room: plugin install caches content, so a rule added to `/custom/` would not be seen until reinstall (`docs/customizing-bcquality.md`, "Use your fork").

### Non-Claude agent (e.g. Copilot agent mode) reading only `AGENTS.md`

Direct reading. `AGENTS.md` gives the agent the prompt that `docs/agent-consumption.md` documents, filled in for the room:

```text
BCQuality root: <repo root>
Review input: file-path = <seeded file paths>   (or folder-path = workshop/AmuseYou/...)
Read BCQuality's skills/entry.md and follow it with this task context:
task-context:
  goal: <fixed goal string, see open point A>
  inputs-available: [file-path]
  technologies: [al]
  enabled-layers: [microsoft, community, custom]
  disabled-skills: []
Return each complete findings report unchanged.
```

Notes:
- `bc-version` is omitted on purpose (docs say omit rather than guess). Files with non-`all` versions become "conditionally applicable" and findings from them are capped at `medium` confidence. Fine for the room.
- Entry's Preparation runs `pwsh ./tools/Build-KnowledgeIndex.ps1` (sub-second). **Without `pwsh` the review still works**: READ falls back to path-based discovery. `knowledge-index.json` is gitignored, so it never pollutes a PR.
- Entry routes on the `goal` text. The agent must copy the goal verbatim, so `AGENTS.md` fixes it.

### Open point A: review scope vs. room time (needs your call)

A broad goal dispatches the super-skill `microsoft/skills/review/al-code-review.md`, which runs **19 domain leaves** over **~435 articles** plus a cross-cutting self-review pass. On a mixed-agent laptop room that is likely many minutes per pair, and slower agents may return `partial`.

Entry dispatches the **narrowest skill that matches the goal** (`entry.md`, Worklist step 2). So a goal naming only the card domains, e.g. *"Review style, testing, performance, security and upgrade concerns in these AL files without changing them"*, should dispatch just those 5 leaves. **Recommendation:** use that narrowed goal for Lab 1 / reruns, and scope input to the seeded files (`file-path`) rather than the whole app. To verify in the Monday end-to-end test.

## 2. Where `/custom/` lives and how it is picked up

- Layout: `custom/knowledge/<domain>/<slug>.md` for rules, `custom/skills/` for action skills (`custom/README.md`). Both currently hold only `.gitkeep`.
- **Use an existing domain folder name.** A new domain needs its own action skill to be discovered (`docs/customizing-bcquality.md`). Domains matching the card categories:

  | Card category | BCQuality domain folder | Leaf that reads it |
  |---|---|---|
  | code quality | `style` | `al-style-review` |
  | testability | `testing` | `al-testing-review` |
  | performance | `performance` | `al-performance-review` |
  | security | `security` | `al-security-review` |
  | upgradeability | `upgrade` | `al-upgrade-review` |

- Each leaf's Source step reads its domain "across enabled layers", so a file in `custom/knowledge/performance/` is seen by `al-performance-review` with no config. All three layers are on by default.
- Index scope: `tools/Build-KnowledgeIndex.ps1` walks only `{microsoft,community,custom}/knowledge/**/*.md`. So **anything under `workshop/` is invisible to the review** (cards, fallbacks, planted rules in `workshop/.notes/planted/`). Good: planted rules stay inert until moved.
- `custom/_TEMPLATE.md` (brief 5.8) sits outside `custom/knowledge/`, so it is not indexed either. Good.
- **Precedence:** Custom > Community > Microsoft, but only when applicability overlaps **and** `## Best Practice` / `## Anti Pattern` text contradicts. Otherwise additive. The losing article is listed in the report's `suppressed`. Stance rules that loosen a Microsoft rule ("a long lock is OK here") therefore work as real overrides, and show up in `suppressed`, which is a nice Lab 2 talking point.
- Index freshness: Entry rebuilds the index before routing, so a rerun after "Write our rule" picks up the new file. The plugin route would not (cache).

## 3. Knowledge file schema (READ v1)

Path: `<layer>/knowledge/<domain>/<slug>.md`. Frontmatter, **all six fields required**, missing/empty = file is invalid and silently skipped:

```yaml
---
bc-version: [all]            # or [26, 27], [26..28], [26..]
domain: performance          # single string, should match the folder
keywords: [lock, user-action, posting]   # 3-10, lowercase kebab-case
technologies: [al]           # no 'all' sentinel allowed
countries: [w1]              # w1 = worldwide
application-area: [all]
---
```

Body:
- `# Title`
- `## Description`: required. The concern and why it matters (primary retrieval target).
- `## Best Practice` and `## Anti Pattern`: optional but the **only normative sections**. The rule's actual stance must be here or it will not drive findings or overrides.
- Other `##` sections are allowed and non-normative.
- **No fenced code blocks.** Code goes in sibling files `<slug>.good.al` / `<slug>.bad.al`, linked as `` [`<slug>.bad.al`](<slug>.bad.al) ``.

Worked example in `docs/customizing-bcquality.md` ("Add an organization-specific rule").

Local validator: `python .github/scripts/validate_frontmatter.py --root .` (needs `pyyaml`). Not required in the room. The gate could optionally do a pwsh re-implementation of the six-field check.

## 4. Findings-report facts relevant to the gate

- `outcome`: `completed | not-applicable | no-knowledge | partial | failed`. Docs: `partial` / `failed` are **not** a clean result.
- `summary` has integer counts `blocker`, `major`, `minor`, `info`.
- Agent findings (no curated rule behind them) are **capped at `minor`** (`do.md`, "Agent findings"). So **a blocker can only come from a knowledge file**, i.e. a BCQuality or `/custom/` rule. That makes "zero blockers" a meaningful gate, and lets a pair's own rule create a blocker.

## 5. Things that would have bitten us

- Upstream `guard-custom-layer.yml` auto-closes any PR adding files under `custom/`, and `flag-new-top-level.yml` comments on new root entries like `workshop/` or `AGENTS.md`. Both are guarded by `github.repository == 'microsoft/BCQuality'` so they would not have fired, but they are removed anyway (see `UPSTREAM.md`).
- `CODEOWNERS` names Microsoft maintainers. Left untouched (BCQuality content). GitHub ignores owners without write access, so no review requests are sent. It may show a CODEOWNERS warning in the file view.
- No root `AGENTS.md`, `CLAUDE.md` or `copilot-instructions.md` exists upstream, so a root `AGENTS.md` does not collide.
- `.gitignore` ignores `.vscode/`. Any workshop VS Code settings would need a negation rule, which means touching a BCQuality file. Not needed so far.
