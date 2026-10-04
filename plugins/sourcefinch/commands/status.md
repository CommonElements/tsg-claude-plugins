---
description: Show your SourceFinch plan, usage and the health of your monitored sources
allowed-tools: mcp__plugin_sourcefinch_sourcefinch__whoami, mcp__plugin_sourcefinch_sourcefinch__get_usage, mcp__plugin_sourcefinch_sourcefinch__find_sources, mcp__plugin_sourcefinch_sourcefinch__list_runs
---

Give a short SourceFinch status report using the SourceFinch MCP tools:

1. `get_usage` (or `whoami` if `get_usage` isn't available): project, plan (including a trial and when it ends), runs used against the allowance, and source limits.
2. `find_sources` with an empty or broad query to list the monitored sources.
3. `list_runs` for recent runs. Flag sources whose latest run is `failed`, `degraded` or `blocked`, and say why in one line each.

Output: a compact table of sources (name, schedule, last run status and time), then one line of plan and usage, then any action needed. If the tools aren't connected, tell the user to run `/mcp` and sign in to **sourcefinch**.
