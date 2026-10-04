# Voydar for Claude Code

Maritime intelligence from [Voydar](https://www.voydar.com) inside Claude Code: vessel profiles and tracks, port calls and arrivals, company fleets and ownership, and sanctions screening, each with its sources.

The plugin bundles the Voydar MCP server (`https://www.voydar.com/mcp`, OAuth) and adds:

| Skill / command | What it does |
|---|---|
| `track-vessel` | Resolve a ship, profile, recent track and port calls; how to set arrival alerts |
| `port-briefing` | Port arrivals, harbour details and warnings; canal and strait transit counts |
| `company-fleet` | Legal entity, corporate structure and fleet by role, with evidence |
| `sanctions-check` | Screening against OFAC SDN, UK, Canada and EU lists, plus a sourced vessel brief |
| `/voydar:usage` | Plan, credits and commercial-use status |

A free Voydar account works, and new accounts get 14 days of Pro free with no card. On first use, run `/mcp`, choose **voydar** and sign in. Tool calls cost credits on your plan (most are 1 credit; usage is free). Data whose source doesn't permit API use is reported as withheld.
