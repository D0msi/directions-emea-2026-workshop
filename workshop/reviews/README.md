# Review reports

Each "Run the BCQuality Review" writes one report here: `workshop/reviews/<slug>.md`, where `<slug>` is your pair name or topic (lowercase, dashes).

The format is defined in [`AGENTS.md`](../../AGENTS.md#review-report-contract). The PR gate reads the frontmatter: no report, `blockers` above 0, or `outcome: failed` turns the check red.
