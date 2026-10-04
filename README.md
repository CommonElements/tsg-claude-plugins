# The Schoeller Group plugins for Claude Code

A Claude Code plugin marketplace for The Schoeller Group's products.

| Plugin | Product | What it adds |
|---|---|---|
| [`sourcefinch`](plugins/sourcefinch) | [SourceFinch](https://sourcefinch.com): monitoring of public web sources with evidence | The SourceFinch MCP server plus skills to monitor a source, review changes, export evidence and set up webhook delivery |
| [`voydar`](plugins/voydar) | [Voydar](https://www.voydar.com): maritime intelligence | The Voydar MCP server plus skills to track a vessel, brief on a port, look up a company fleet and screen for sanctions |

## Install

In your shell:

```bash
claude plugin marketplace add CommonElements/tsg-claude-plugins
claude plugin install voydar@tsg-plugins
claude plugin install sourcefinch@tsg-plugins
```

Or from inside Claude Code: `/plugin install voydar --marketplace CommonElements/tsg-claude-plugins`.

Then run `/mcp`, choose the server, and sign in (OAuth). To receive new versions automatically, open `/plugin` → **Marketplaces** → `tsg-plugins` and turn on auto-update. Otherwise run `claude plugin update <name>@tsg-plugins`.

Notes:
- **Voydar** works with a free account, and new accounts get 14 days of Pro, no card. Tool calls use your plan's credits.
- **SourceFinch** is an invite-only beta; request access at https://sourcefinch.com/request-access.

## Releasing (maintainers)

1. Change the plugin under `plugins/<name>/`.
2. Bump `version` in `plugins/<name>/.claude-plugin/plugin.json`. The version lives **only** in `plugin.json`, never in `marketplace.json`. Users stay on the old copy until it changes.
3. `claude plugin validate .` and `claude plugin validate --strict plugins/<name>` must pass. CI runs both.
4. Install from your local checkout (`claude plugin marketplace add ./`) and confirm the skills and the MCP server load.
5. Open a PR and merge it. Optionally run `claude plugin tag --push` from the plugin directory.

Never rename a published plugin. If a rename is unavoidable, add the old name to `renames` in `marketplace.json` (`{"old-name": "new-name"}`). The map is append-only: never remove an entry, or users of the old name lose the plugin.

## License

MIT. The products themselves are covered by their own terms: [SourceFinch](https://sourcefinch.com/terms), [Voydar](https://www.voydar.com/terms).

## OpenAI packages

`openai/<product>/` holds Codex-format plugin packages for the ChatGPT app directory (with the review test cases).
The ZIPs are in `dist/`. See `docs/DIRECTORY_SUBMISSIONS.md`.
