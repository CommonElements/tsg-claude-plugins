---
name: review-changes
description: Review and explain what changed in the user's SourceFinch monitors since the last run or over a period. Use for "what changed", "any updates on my sources", "summarize this week's changes", or before acting on monitored data.
---

# Review and explain SourceFinch changes

1. Find the source. If the user names one, call `find_sources` with the name or URL; otherwise review all sources.
2. Start with `get_changes_digest` (optionally with `since`, an ISO date, and a `source_id`). One call returns, per source, the validated runs, the records added, removed and changed, the fields that moved and a few before/after examples. A source's first run is its baseline and is reported apart from additions. If the server doesn't offer that tool, or the user wants every change, call `get_changes` (optionally with the source id) and page with `cursor` until you have the period asked about.
3. To find a particular value across the current records ("which licences expire in March?"), call `search_records` with the words to match; `field` limits the match to one field. It says how many records it scanned and whether it stopped early.
4. Summarize for a person, not a log:
   - lead with the count of added, removed and changed records per source;
   - then the changes that matter, quoting the field, the old value and the new value;
   - group repetitive changes (for example "42 prices moved by under 1%") instead of listing them all.
5. If a run is **degraded** (quality checks failed and its output is held for review), say so plainly. Call `get_run` to show why. Only if the user confirms the source really changed, call `accept_run`; it becomes the new baseline. Never accept a degraded run on your own judgment.
6. If the user asks "how do we know", point to the evidence: each record has an evidence id, and `verify_evidence` checks a run's capture hashes, signature and timestamp. The **export-evidence** skill shows how to fetch and verify it.

Don't invent explanations for why something changed. Report what the data shows and say when the cause is unknown.
