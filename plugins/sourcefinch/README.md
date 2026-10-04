# SourceFinch for Claude Code

Monitor public web sources from Claude Code with [SourceFinch](https://sourcefinch.com): find a Verified source, set up a monitor, review what changed, export the evidence behind any value, and deliver changes to your own system by signed webhook.

The plugin bundles the SourceFinch MCP server (`https://sourcefinch.com/mcp`, OAuth) and adds:

| Skill / command | What it does |
|---|---|
| `monitor-source` | Catalog → rights check → monitor → baseline → schedule |
| `review-changes` | Explains what changed since the last run or over a period |
| `export-evidence` | CSV/JSON exports, captured evidence, signed manifests and WACZ bundles |
| `setup-delivery` | Signed webhook destinations, tests and delivery receipts |
| `/sourcefinch:status` | Plan, usage and the health of your sources |

SourceFinch is an **invite-only beta**; request access at https://sourcefinch.com/request-access. On first use, run `/mcp`, choose **sourcefinch** and sign in.

SourceFinch respects robots.txt and never bypasses logins, paywalls or bot challenges. See https://sourcefinch.com/terms and https://sourcefinch.com/privacy.
