# Review reports

Each "Run the BCQuality Review" writes one report here: `workshop/reviews/<pair-slug>.md`, where `<pair-slug>` is your pair name (lowercase, dashes). The "Write our rule" rerun writes it, and "Submit our rule" commits that same file as the receipt.

The format is defined in [`AGENTS.md`](../../AGENTS.md#review-report-contract). The PR gate reads the frontmatter: no report, `blockers` above 0, or `outcome: failed` turns the check red.
