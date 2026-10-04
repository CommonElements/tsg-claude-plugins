# Portfolio MCP Distribution Playbook (v1.0)

**Status:** canonical for every TSG product that ships an MCP server. **Authored:** 2026-10-04, from doing it for
SourceFinch and Voydar. Companion to `PORTFOLIO_ENGINEERING_STANDARD.md` and `PORTFOLIO_TOOLING.md`. A copy lives in
`CommonElements/tsg-claude-plugins/docs/`; this file in `~/dev` is the source.

Common Elements products are **not** in the TSG marketplace or under TSG namespaces; CE runs its own (entity
separation, `PORTFOLIO_TOOLING.md` rule 1).

## 1. The server: requirements before any listing

| Requirement | How we do it |
|---|---|
| Remote **Streamable HTTP** at a stable public URL on the product's own domain | `https://<domain>/mcp`; POST JSON-RPC; GET returns 405 to clients and **redirects browsers** (`Accept: text/html`) to the docs page |
| **OAuth 2.1** per the MCP auth spec | RFC 9728 protected-resource metadata at `/.well-known/oauth-protected-resource` (and `/…/mcp`); RFC 8414 at `/.well-known/oauth-authorization-server`; **dynamic client registration**; authorization code + **PKCE S256**; public clients (`token_endpoint_auth_methods_supported: none`); rotating refresh tokens; revocation endpoint; honour the `resource` parameter |
| Unauthenticated `initialize` → **401** with `WWW-Authenticate` pointing at the resource metadata | Directory crawlers and clients discover auth this way |
| An **API-key path** for headless agents | `Authorization: Bearer <key>` alongside OAuth |
| **Scopes** that mean something | Read vs write (SourceFinch `read`/`write`; Voydar `mcp`/`follow`); a consent page that names the project/account and the scope; a "Connected apps" page to revoke |
| Honest **tool design** | Every tool has `readOnlyHint` (and `destructiveHint`/`idempotentHint`/`openWorldHint` where they apply); write tools hidden from read-only connections; descriptions say what it does and what it costs; data the source doesn't license is returned as **withheld**, never guessed |
| **security.txt** | `/.well-known/security.txt` with a security@ contact (SourceFinch ✔; Voydar ✗ as of 2026-10-04) |
| Reviewed **terms, privacy, AUP** at stable URLs | Directories link to them; "pending legal review" banners block the Anthropic directories |
| Status / health | A public `/status` and worker heartbeat alerting |

## 2. Official MCP Registry (registry.modelcontextprotocol.io)

The registry is the upstream that PulseMCP, Glama and others ingest, so list here first.

1. `mcp/server.json` in the product repo (schema `2025-12-11`): `name` = `<reverse-domain>/<product>` (for example
   `com.voydar/voydar`), `title`, `description` (**≤ 100 characters**, honest status included), `version`, `websiteUrl`,
   `icons` (an https SVG or PNG), `remotes: [{ "type": "streamable-http", "url": "https://<domain>/mcp" }]`.
2. **Namespace proof by DNS** (our domains are on Vercel DNS, team `theschoellergroup`):
   ```bash
   K=~/.config/<product>; mkdir -p $K && chmod 700 $K
   openssl genpkey -algorithm Ed25519 -out $K/mcp-registry-ed25519.pem && chmod 600 $K/mcp-registry-ed25519.pem
   PUB=$(openssl pkey -in $K/mcp-registry-ed25519.pem -pubout -outform DER | tail -c 32 | base64)
   vercel dns add <domain> @ TXT "v=MCPv1; k=ed25519; p=$PUB" --scope theschoellergroup
   ```
   The key stays on Harry's Mac (never committed). Record the TXT value in the product's `docs/DISTRIBUTION.md`.
3. Get `mcp-publisher` (GitHub releases of `modelcontextprotocol/registry`, darwin_arm64), then:
   ```bash
   PRIV=$(openssl pkey -in $K/mcp-registry-ed25519.pem -noout -text | grep -A3 "priv:" | tail -n +2 | tr -d ' :\n')
   mcp-publisher validate mcp/server.json
   mcp-publisher login dns --domain <domain> --private-key "$PRIV"
   mcp-publisher publish mcp/server.json
   ```
4. Verify: `GET https://registry.modelcontextprotocol.io/v0/servers?search=<namespace>` → status `active`.
5. New versions: bump `version`, publish again. When an npm stdio bridge exists, add a `packages` entry
   (`registryType: npm`, `transport: stdio`, env vars with `isSecret`) **after** the package is on npm (the registry
   checks it), and set `"mcpName": "<registry name>"` in that package's `package.json`.

## 3. Directories and their eligibility rules

| Directory | How | Eligibility |
|---|---|---|
| Official MCP Registry | `mcp-publisher` (above) | Anyone with the namespace; say "invite-only beta" if it is |
| mcp.so | A GitHub issue on `chatmcp/mcpso` titled `Submit Remote MCP Server: <Name> (<registry name>)` with URL, docs, tools, auth, config and registry id | Accepts betas when the copy is honest |
| PulseMCP | Ingests the registry (its form is bot-protected) | Check about 48 h after publishing |
| Glama connectors | Ingests the registry by reverse-DNS id; a Glama sign-in can claim it | Needed for the badge below |
| awesome-remote-mcp-servers | PR to `punkpeye/awesome-remote-mcp-servers`; star the repo first; 3-line entry with the Glama badge and 🔓/🔑/🔐 marker | **Public sign-up only** (no invite-only); must answer `initialize`; Glama badge required |
| awesome-mcp-servers | PR to `punkpeye/awesome-mcp-servers` | **Installable servers with a public repo** only (an npm stdio bridge from a public repo); remote-only goes to the list above |
| Smithery, Cursor directory | Web forms | Need Harry's sign-in; prepare the text in DISTRIBUTION.md |
| VS Code / GitHub MCP gallery | Curated from registry listings | Not self-serve |
| Anthropic connector directory | Application | Commits TSG to Anthropic's terms → **Harry submits**. Needs reviewed legal pages, public sign-up, security.txt and a reviewer account |

Rules: no fabricated metrics, reviews or claims; every listing true on the day it's made; no outreach by email or DM.
Directory forms and GitHub PRs to public lists are fine with Harry's approval for that product.

## 4. One-click install formats (put them on the product's MCP docs page)

| Client | Link |
|---|---|
| Cursor | `cursor://anysphere.cursor-deeplink/mcp/install?name=<n>&config=<base64 of {"url":"…"}>` (web fallback `https://cursor.com/install-mcp?name=…&config=…`) |
| VS Code | `https://vscode.dev/redirect/mcp/install?name=<n>&config=<urlencoded {"type":"http","url":"…"}>`. It 302s to `vscode:mcp/install?…`. Insiders: `insiders.vscode.dev/…&quality=insiders` |
| Claude (web, Desktop, mobile) | No deeplink: link `https://claude.ai/settings/connectors` and tell the user to **Add custom connector** with the URL |
| Claude Code | `claude mcp add --transport http <n> https://<domain>/mcp`, or the plugin (below) |
| Any `mcp.json` client | `{"mcpServers":{"<n>":{"type":"http","url":"…"}}}` |

Links use **OAuth** (no key in the URL). Generate them in one tested helper (`lib/mcp/install-links.ts` in each repo),
never hand-written in JSX.

## 5. Claude Code plugin and the TSG marketplace

**One public marketplace for all TSG products:** `CommonElements/tsg-claude-plugins`, marketplace name `tsg-plugins`,
owner "The Schoeller Group". Each product is `plugins/<product>/` with a relative-path `source`.

- `plugins/<p>/.claude-plugin/plugin.json`: `name` (kebab-case, permanent), `displayName`, **`version` (here only, never
  in the marketplace entry; bump it every release)**, `description`, `author`, `homepage`, `repository`, `license`,
  `keywords`, plus the directory listing fields `icon` (an image inside the plugin), `documentationUrl`, `supportUrl`,
  `privacyPolicyUrl`, `termsOfServiceUrl`.
- `plugins/<p>/.mcp.json`: `{"mcpServers":{"<p>":{"type":"http","url":"https://<domain>/mcp"}}}`. It shows up as
  `plugin:<p>:<p>`, so it doesn't collide with a user-scope server of the same name.
- `skills/<name>/SKILL.md` (frontmatter `name`, `description` saying *when* to use it): 3–5 real workflows, written
  against the server's **actual tool names**. Coordinate renames with the MCP owner, and update the skills in the same
  release. Commands in `commands/*.md` for short status-type actions. **No hooks or `bin/` executables** unless clearly
  needed (they don't load on claude.ai/Cowork, and they run code on users' machines).
- `marketplace.json`: `name`, `owner`, `plugins[]` (`name` = the manifest name, `source`, `description`, `category`),
  `"renames": {}`. **`renames` is append-only**: never remove an entry, or users of the old name lose the plugin.
- `relevance` per entry (`topic`, `signals`: `cwd`, `cli`, `hosts`, `filesRead`, `manifestDeps` with **JavaScript** regexes,
  so no `(?m)`; use `(^|\n)`). Suggestions only appear for marketplaces an admin allowlists in managed settings
  (`pluginSuggestionMarketplaces`), so they help orgs that adopt us, not the general public.
- Validate: `claude plugin validate .` and `claude plugin validate --strict plugins/<p>`. CI runs both
  (`.github/workflows/validate.yml`).
- Verify an install **in an isolated config** so you don't touch Harry's setup:
  `CLAUDE_CONFIG_DIR=<scratch> claude plugin marketplace add CommonElements/tsg-claude-plugins` → install →
  `claude plugin details <p>` (skills listed) → `claude mcp list` (server listed) → uninstall → remove the marketplace.
- Users: `claude plugin marketplace add CommonElements/tsg-claude-plugins` then `claude plugin install <p>@tsg-plugins`,
  or `/plugin install <p> --marketplace CommonElements/tsg-claude-plugins`. Tell them to turn on auto-update under
  `/plugin` → Marketplaces. Each product gets a `/docs/claude-code` (or `/developers/mcp`) section with these commands.

## 6. Anthropic plugin directory (claude.ai/directory)

Submit from https://claude.ai/directory/manage (paid claude.ai plan; on Team or Enterprise an Owner). The repo must be on
GitHub. Run `claude plugin validate --strict`, walk claude.com's pre-submission checklist, and check the component
support table (skills and remote MCP load everywhere; hooks and `bin/` don't load on claude.ai/Cowork). Listing
metadata comes from `plugin.json`. A listing reaches claude.ai, Cowork and Claude Code (as `<name>@synced`).
**Harry submits**: it commits TSG to Anthropic's terms. The same readiness gates as the connector directory apply.
The official `claude-plugins-official` marketplace isn't open to submissions (partner contact only).

## 7. Per-product checklist

- [ ] Server meets §1 (OAuth discovery, 401, API key, scopes, annotations, withheld-not-guessed, browser redirect)
- [ ] `/.well-known/security.txt` live
- [ ] Terms, privacy and AUP reviewed (no draft banner)
- [ ] `mcp/server.json` in the repo; DNS TXT added; published; registry shows `active`
- [ ] mcp.so issue filed; PulseMCP and Glama checked after 48 h; Glama claimed
- [ ] awesome-remote-mcp-servers PR (only once sign-up is public and the Glama badge exists)
- [ ] Smithery and Cursor-directory text prepared for Harry
- [ ] Install buttons and Claude Code commands on the product's MCP docs page (tested helper)
- [ ] `plugins/<p>` in `tsg-claude-plugins`: manifest, `.mcp.json`, 3–5 skills, a status command, README, relevance;
      validated; isolated install verified
- [ ] `/docs/claude-code` (or equivalent) page, plus sitemap and llms.txt
- [ ] npm stdio bridge (optional) published by Harry; registry `packages` entry added afterwards; awesome-mcp-servers PR
- [ ] Anthropic connector and plugin directory packages prepared; Harry submits when the gates are met
- [ ] Every listing recorded in the product's `docs/DISTRIBUTION.md` (where, when, link, status)

## 8. Status (2026-10-04)

| Product | Registry | mcp.so | Plugin | Install buttons | Anthropic directories |
|---|---|---|---|---|---|
| SourceFinch | `com.sourcefinch/sourcefinch` live | #4731 | `sourcefinch@tsg-plugins` 0.1.0 | /docs/mcp, /docs/claude-code | At public launch (invite-only; legal drafts) |
| Voydar | `com.voydar/voydar` live | #4732 | `voydar@tsg-plugins` 0.1.0 | /developers/mcp | Public sign-up ✔, but the legal pages say "pending legal review" and there's no security.txt. Fix both, then Harry submits |

## Entilume: future

Entilume has **no MCP server yet**, and shouldn't get one until there is something true and licensable to serve. First:

1. **A public projection.** Today 0 sources are publication-enabled and 0 public rows exist. MCP tools would read
   from that projection only, never from raw acquisition tables.
2. **Rights-qualified data.** Every served field needs documented publication and API rights (the same
   rights matrix discipline as Voydar's `withheld` sections). Sources without API rights are withheld.
3. **Entity linkage** good enough to answer "who is this business" (the GLEIF ↔ registry crosswalk) with evidence per link.
4. **Accounts and limits**: auth, an API-key and OAuth model, plans and credits, before a public endpoint exists.

Then follow this playbook: namespace `com.entilume/entilume` (DNS on entilume.com), and `plugins/entilume` in the TSG
marketplace with skills such as "look up a business entity", "trace ownership/officers" and "verify a registration",
each grounded in real tools.
