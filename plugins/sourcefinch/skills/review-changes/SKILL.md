---
name: review-changes
description: Review and explain what changed in the user's SourceFinch monitors since the last run or over a period. Use for "what changed", "any updates on my sources", "summarize this week's changes", or before acting on monitored data.
---

# Review and explain SourceFinch changes

1. Find the source. If the user names one, call `find_sources` with the name or URL; otherwise review all sources.
2. Call `get_changes` (optionally with the source id). It returns records added, removed or changed between validated runs, with the changed fields and the before and after values. Page with `cursor` until you have the period the user asked about.
3. Summarize for a person, not a log:
   - lead with the count of added, removed and changed records per source;
   - then the changes that matter, quoting the field, the old value and the new value;
   - group repetitive changes (for example "42 prices moved by under 1%") instead of listing them all.
4. If a run is **degraded** (quality checks failed and its output is held for review), say so plainly. Call `get_run` to show why. Only if the user confirms the source really changed, call `accept_run`; it becomes the new baseline. Never accept a degraded run on your own judgment.
5. If the user asks "how do we know", point to the evidence: each record has an evidence id. The **export-evidence** skill shows how to fetch and verify it.

Don't invent explanations for why something changed. Report what the data shows and say when the cause is unknown.
