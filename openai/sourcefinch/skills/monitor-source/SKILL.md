---
name: monitor-source
description: Start monitoring a public web page, feed or dataset with SourceFinch. Use when the user wants to track a public source over time, get notified when it changes, or turn a page into structured records — "monitor X", "watch this page for changes", "track the FDA recalls list".
---

# Monitor a public web source with SourceFinch

SourceFinch turns a public page, feed or API into structured records, re-checks it on a schedule, and keeps hashed evidence of every value. These steps use the tools of the SourceFinch MCP server bundled with this plugin. If its tools aren't available, ask the user to connect SourceFinch and sign in.

1. **Check the plan first.** Call `get_usage` (if the server offers it) or `whoami` to see the plan, remaining runs and source limits. If the plan has no room for another source, say so plainly (which limit is reached) instead of failing later. Don't link to upgrades or checkout.
2. **Prefer a Verified catalog source.** Call `search_catalog` with the topic. If a Verified source fits, call `get_catalog_source` and show the user its fields, a few sample records, the 30-day reliability and its usage rights.
3. **Confirm the rights.** Usage rights are documented provenance and terms at capture time, not legal advice. Ask the user to confirm they have the right to collect this data before you create anything. Never set `rights: true` on their behalf without that confirmation.
4. **Create the monitor.**
   - For a catalog source, call `monitor_catalog_source` with the `slug` and `rights: true` (and an optional `name` and `schedule`). This pins the Verified recipe version and its usage rights. If that tool isn't available, call `create_source` with the `create_source_args` from `get_catalog_source`.
   - For a URL that isn't in the catalog, call `create_source` with `dry_run: true` first and show the user the preview records. Fix selectors until the preview is right, then call it again without `dry_run`.
5. **Take the baseline.** Call `run_source` with `wait_seconds: 60`. Report the result with `get_run`: status, record count and anything held for review.
6. **Set the schedule** if the user wants one different from the default, with `update_source` (`manual`, `hourly`, `daily` or `weekly`; the plan limits the fastest schedule).
7. Finish with what happens next: when the next check runs, and that the **review-changes** skill shows what changed.

Rules: SourceFinch respects robots.txt and never bypasses logins, paywalls or bot challenges, whoever asks. Don't suggest workarounds. Runs count against the plan; failed runs are free.
