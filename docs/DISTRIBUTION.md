# Distribution record: TSG Claude Code plugins

The marketplace: https://github.com/CommonElements/tsg-claude-plugins (public), name `tsg-plugins`. The playbook is
in [PORTFOLIO_MCP_DISTRIBUTION.md](PORTFOLIO_MCP_DISTRIBUTION.md). Each product's server listings are recorded in its
own repo's `docs/DISTRIBUTION.md` (SourceFinch) or below (Voydar).

| Plugin | Version | Verified (UTC) | Notes |
|---|---|---|---|
| `sourcefinch` | 0.1.0 | 2026-10-04: installed from GitHub in a clean config; 5 skills and `plugin:sourcefinch:sourcefinch` MCP loaded; test install removed | Invite-only beta |
| `voydar` | 0.1.0 | 2026-10-04: same check; 5 skills and `plugin:voydar:voydar` MCP loaded; test install removed | Public; free account; 14-day Pro trial, no card |

## Voydar server listings

| Directory | Date | Link | Status |
|---|---|---|---|
| Official MCP Registry | 2026-10-04 | https://registry.modelcontextprotocol.io/v0/servers?search=com.voydar | **Live**: `com.voydar/voydar` 1.0.0 active. Namespace by DNS TXT on voydar.com (`v=MCPv1; k=ed25519; p=jXhD6S3oefaswuVF97SrsOmolF6J0ddP1+aN8wGEb0E=`); key at `~/.config/voydar/mcp-registry-ed25519.pem` on Harry's Mac. `mcp/server.json` is in the Voydar repo |
| mcp.so | 2026-10-04 | https://github.com/chatmcp/mcpso/issues/4732 | Submitted |
| PulseMCP, Glama | — | — | Ingest from the registry; check after 48 h, then claim on Glama |
| awesome-remote-mcp-servers | — | — | **Eligible** (public sign-up, OAuth, answers `initialize`) once the Glama connector exists. Entry below |
| Smithery, Cursor directory | — | — | Need Harry's sign-in (same text as below) |
| Install buttons | 2026-10-04 | https://www.voydar.com/developers/mcp | Voydar PR #81 |

Entry for awesome-remote-mcp-servers (category: data / maritime):

```markdown
- [Voydar](https://www.voydar.com/developers/mcp) `https://www.voydar.com/mcp`
  [![Voydar MCP connector](https://glama.ai/mcp/connectors/com.voydar/voydar/badges/score.svg)](https://glama.ai/mcp/connectors/com.voydar/voydar)
  🔐 - Vessel profiles and tracks, port calls, company fleets and sanctions screening, each with its sources.
```

## Anthropic plugin directory

Submit from https://claude.ai/directory/manage (Harry; paid claude.ai plan). Repository `CommonElements/tsg-claude-plugins`,
paths `plugins/voydar` and `plugins/sourcefinch`. Listing fields are already in each `plugin.json`.

- **Voydar: could go now, except for two gates.** Public sign-up ✔, OAuth ✔, honest withheld data ✔. But
  https://www.voydar.com/terms and /privacy say the text is "drafted for The Schoeller Group LLC and is pending legal
  review", and there is no `/.well-known/security.txt`. Clear both, then submit.
  - Copy: "Voydar brings sourced maritime intelligence into Claude: look up vessels by name, IMO or MMSI, see recent
    tracks and port calls, brief on a port's arrivals, trace company fleets and ownership, and screen vessels against
    OFAC, UK, Canadian and EU sanctions lists. Data a source doesn't license for API use is shown as withheld, never guessed."
- **SourceFinch: at public launch.** It's invite-only, and the legal pages are also pending review. Its prepared copy is
  in the SourceFinch repo's `docs/DISTRIBUTION.md`.

## Claude and OpenAI directories

Prepared 2026-10-04; Harry submits (each portal needs his login and accepts terms in TSG's name). Full packages, copy,
reviewer steps and click-through: [DIRECTORY_SUBMISSIONS.md](DIRECTORY_SUBMISSIONS.md). OpenAI ZIPs are in `dist/`,
built from `openai/<product>/` (`cd openai/<p> && zip -qr ../../dist/<p>-openai-<version>.zip . -x '.DS_Store'`).
