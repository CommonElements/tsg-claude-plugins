---
name: monitor-source
description: Start monitoring a public web page, feed or dataset with SourceFinch. Use when the user wants to track a public source over time, get notified when it changes, or turn a page into structured records — "monitor X", "watch this page for changes", "track the FDA recalls list".
---

# Monitor a public web source with SourceFinch

SourceFinch turns a public page, feed or API into structured records, re-checks it on a schedule, and keeps hashed evidence of every value. These steps use the tools of the SourceFinch MCP server bundled with this plugin. If its tools aren't available, tell the user to run `/mcp`, choose **sourcefinch** and sign in (an account is free at https://sourcefinch.com/signup).

1. **Check the plan first.** Call `get_usage` (if the server offers it) or `whoami` to see the plan, remaining runs and source limits. If the plan has no room for another source, say so and link the `upgrade_url` instead of failing later.
2. **Prefer a Verified catalog source.** Call `search_catalog` with the topic. If a Verified source fits, call `get_catalog_source` and show the user its fields, a few sample records, the 30-day reliability and its usage rights.
3. **Confirm the rights.** Usage rights are documented provenance and terms at capture time, not legal advice. Ask the user to confirm they have the right to collect this data before you create anything. Never set `rights: true` on their behalf without that confirmation.
4. **Create the monitor.**
   - For a catalog source, call `monitor_catalog_source` with the `slug` and `rights: true` (and an optional `name` and `schedule`). This pins the Verified recipe version and its usage rights. If that tool isn't available, call `create_source` with the `create_source_args` from `get_catalog_source`.
   - For a URL that isn't in the catalog, call `preview_source` with the URL first. It fetches one sample (robots.txt first), creates nothing and uses no run, and returns the records it found with a proposed recipe, record key and sample rows, plus any feed or API that carries the same data (usually the better source). Show the user the sample rows. Then call `create_source` with the returned `create_source_args` plus `rights: true` once they have confirmed their right to collect it; keep `dry_run: true` for a first validation and call it again without. If `preview_source` isn't available, use `create_source` with `dry_run: true` and adjust selectors until the preview is right.
5. **Take the baseline.** Call `run_source` with `wait_seconds: 60`. Report the result with `get_run`: status, record count and anything held for review.
6. **Set the schedule** if the user wants one different from the default, with `update_source` (`manual`, `hourly`, `daily` or `weekly`; the plan limits the fastest schedule).
7. **Stopping or removing a monitor.** `pause_source` stops the schedule and keeps everything. `delete_source` removes the source (it needs `confirm: true`, so ask the user first): it can be brought back with `restore_source` for 14 days, and its records and evidence are removed after the plan's retention period.
8. Finish with what happens next: when the next check runs, and that `/sourcefinch:status` or the **review-changes** skill shows what changed.

Rules: SourceFinch respects robots.txt and never bypasses logins, paywalls or bot challenges, whoever asks. Don't suggest workarounds. Runs count against the plan; failed runs are free.
