# Claude and OpenAI directory submissions: Voydar and SourceFinch

Prepared 2026-10-04. Harry approved applying ("We'd love to get them on the official Claude and OpenAI connector
directories"). **Every submission below needs Harry's own login**: each portal belongs to his account and organization,
and submitting accepts the directory's terms in TSG's name. So everything is prepared and the click-through steps are
written out; nothing has been submitted yet. Record each submission's date and status in the table at the end.

## Readiness

| Gate | Voydar | SourceFinch |
|---|---|---|
| Remote HTTPS MCP, OAuth with dynamic client registration and PKCE | ✔ `https://www.voydar.com/mcp` | ✔ `https://sourcefinch.com/mcp` |
| Every tool has `title` + `readOnlyHint`/`destructiveHint` (Claude) | ✔ | ✔ |
| All three hints explicit on every tool (OpenAI: `readOnlyHint`, `destructiveHint`, `openWorldHint`) | ✔ | ⚠ read tools omit `destructiveHint` (implied false); sent to the SourceFinch MCP owner to make it explicit |
| security.txt | ✔ (Voydar #82) | ✔ |
| Terms and privacy | ⚠ The current pages say "pending legal review". The full suite is drafted in **Voydar PR #84**, and Harry is approving its decisions. Submit once #84 is live, or say so in the application | ⚠ Same: drafts pending review |
| Sign-up | Public, free account, 14-day Pro trial with no card | **Invite-only beta**. Claude's form asks "what users need before they can connect", so state the invite. OpenAI treats B2B and limited access case by case |
| Reviewer test account | Harry creates one (password login, **no MFA**, no email code) with follows, a watchlist and alerts populated | Harry creates one (password login, no MFA) with 2–3 monitors, a few runs, one change and a test webhook destination |
| OpenAI domain verification | Route ready: `/.well-known/openai-apps-challenge` serves `OPENAI_APPS_CHALLENGE` (Voydar #85) | Same (SourceFinch #85) |
| OpenAI "no upgrade promotion / checkout links" | ✔ Skills don't promote upgrades. Check that `get_usage` text doesn't push upgrades | Skills in the OpenAI package drop the `upgrade_url` link. ⚠ `get_usage` returns `upgrade_url` (a field, not a prompt; mention it in review notes) |

## 1. Anthropic connector directory (both products)

Anthropic asks for **two submissions per product**: the MCP server as an **MCP connector**, and the plugin bundle
(`plugins/<product>` in this repo). They're then paired. Plan: Pro, Max, Team or Enterprise; on Team or Enterprise an
Owner submits. Every listing starts as a **Community** connector after an automated policy scan, and Anthropic may
escalate it to Verified review.

### Click-through (Harry, about 15 minutes per product)

1. **Test first:** in claude.ai, go to Settings → Connectors → **Add custom connector**, paste the server URL, sign in,
   and call each tool once in a chat. (The portal asks you to confirm you did this.)
2. Open https://claude.ai/directory/manage → **Submit new** → **MCP connector**.
3. **Connection:** paste the URL (`https://www.voydar.com/mcp` or `https://sourcefinch.com/mcp`). Users connect to one URL.
4. **Tools:** they sync automatically. Expect no flags.
5. **Listing:** paste from the copy below; the icon is `openai/<product>/assets/logo.png`. The slug (`voydar` /
   `sourcefinch`) is permanent.
6. **Use cases:** paste below. What users need: Voydar: "a free Voydar account"; SourceFinch: "a SourceFinch account
   (invite-only beta; request access at sourcefinch.com/request-access)". Reads and writes: Voydar **reads**;
   SourceFinch **reads and writes** (creates monitors, runs and webhook destinations).
7. **Company:** The Schoeller Group LLC, https://theschoellergroup.com, primary contact Harry.
8. **Authentication:** **OAuth with dynamic client registration**.
9. **Data handling:** Voydar: "our own API; it aggregates public and licensed maritime sources, and data without API
   rights is withheld". SourceFinch: "our own API". No health data, no sponsored content.
10. **Test & launch:** paste the reviewer account's credentials and the steps below. Tick "I ran every tool".
11. **Compliance:** the seven acknowledgments. Read them: they accept the Software Directory Terms and Policy in TSG's name.
12. **Review and submit.** Then **Submit new** → **Plugin bundle** → repo `CommonElements/tsg-claude-plugins`, folder
    `plugins/<product>`, and pair it with the connector.

### Copy: Voydar (Claude)

- **Name:** Voydar
- **One-liner (≤200):** Sourced maritime intelligence: vessels, AIS tracks, port calls, company fleets and sanctions screening, with the source behind every fact.
- **Description (≤2,000):** Voydar brings sourced maritime intelligence into Claude. Look up any vessel by name, IMO or MMSI: identity, owner, manager and operator with evidence, US Coast Guard inspections, EU MRV emissions, sanctions designations, other names, recent AIS positions and port calls. Brief on a port by UN/LOCODE (harbour details, navigational warnings and recent arrivals), see transit counts for canals and straits such as Suez, Panama and Gibraltar, look up a shipping company's legal entity (GLEIF/LEI) and its fleet by role, and screen up to 50 vessels at a time against the OFAC SDN, UK, Canadian and EU sanctions lists. Every answer carries its sources; data whose source doesn't permit API use is reported as withheld, never guessed. Positions come from terrestrial AIS and reported sources, so ships far from shore can be hours or days old, and Voydar says how old each one is. Sanctions screening matches by IMO or MMSI only and is screening support, not legal advice. Works with a free Voydar account; new accounts get 14 days of Pro, no card.
- **Categories:** Research; Data & analytics.
- **Docs:** https://www.voydar.com/developers/mcp. **Privacy:** https://www.voydar.com/privacy. **Support:** the
  Voydar support address from the legal suite (PR #84).
- **Use cases:** vessel due diligence; tracking ships and their port calls; port and chokepoint activity briefings;
  ownership and fleet research; sanctions screening.
- **Reviewer steps:** sign in with the test account at the OAuth prompt, then ask: "Who owns the EVER GIVEN?", "Brief me on
  NLRTM", "Screen IMO 9166778 and 9811000 for sanctions", "Show the fleet of the EVER GIVEN's manager", "What's my Voydar usage?"
- **Note for reviewers:** the legal pages are being finalized (Harry is approving the decisions in the drafted suite);
  the current terms and privacy apply in the meantime.

### Copy: SourceFinch (Claude)

- **Name:** SourceFinch
- **One-liner:** Evidence-grade monitoring of public web sources: catalog, scheduled monitors, field-level changes, exports and signed evidence.
- **Description:** SourceFinch turns public web pages, feeds and datasets into structured records, re-checks them on a schedule, and keeps hashed, signed, timestamped evidence of every value. With the connector, Claude can search a catalog of Verified public sources, start monitoring one (pinned to the reviewed recipe and its usage rights), run it, explain exactly which records were added, changed or removed, export records as CSV, JSON or NDJSON, fetch the captured page behind any value or the offline-verifiable WACZ evidence bundle, and deliver changes to your own system by signed webhook. SourceFinch only collects public sources, respects robots.txt and never bypasses logins, paywalls or bot challenges. Usage rights are documented provenance at capture time, not legal advice. SourceFinch is an invite-only beta; runs count against the workspace's plan.
- **Categories:** Data & analytics; Research; Developer tools.
- **Docs:** https://sourcefinch.com/docs/mcp. **Privacy:** https://sourcefinch.com/privacy. **Support:** hello@sourcefinch.com.
- **Reviewer steps:** sign in at the OAuth prompt and pick the seeded project; ask: "What changed in my sources?", "Search
  the catalog for recalls", "Export the latest run of <seeded source> as CSV", "Show the evidence for its first record",
  "List my webhook destinations and send a test".

## 2. OpenAI app directory (ChatGPT)

Packages, built as **Codex-format plugins** (`.codex-plugin/plugin.json` + `.mcp.json` + `skills/` + `assets/`), are in
`openai/<product>/`. The ready-to-upload ZIPs are `dist/voydar-openai-1.0.0.zip` and `dist/sourcefinch-openai-1.0.0.zip`.
Each manifest has the listing metadata (`interface`) and the required review test cases (5 positive, 3 negative).
Skills are adapted to OpenAI's policy: no upgrade or checkout links.

### Click-through (Harry)

1. **Verify identity:** https://platform.openai.com → Organization settings → complete individual or business
   verification for The Schoeller Group LLC (required to publish). You need to be an org Owner, or have **Apps
   Management Write**.
2. Open https://platform.openai.com/plugins → **Upload new or existing plugin** → upload the ZIP.
3. Fix any automated findings in **Metadata & Skills** and **MCPs**. If the portal expects the test cases somewhere other
   than the manifest's `review.test_cases`, use **Import from manifest** or paste them from the manifest in the dashboard.
4. **Connect the MCP:** URL from `.mcp.json`; auth **OAuth**. If the portal shows a redirect URI to allowlist, both
   servers accept dynamically registered clients, so nothing should be needed. If one is required, send it to the agent.
5. **Domain verification:** the portal shows a token for `https://<host>/.well-known/openai-apps-challenge`. In
   Vercel, set `OPENAI_APPS_CHALLENGE=<token>` on **voydar-web** (host `www.voydar.com`) or **sourcefinch-web**
   (`sourcefinch.com`), Production, then redeploy. Check: `curl https://<host>/.well-known/openai-apps-challenge`
   prints only the token. Then click **Verify**.
6. **Test account:** enter the reviewer credentials (password login, **no MFA, no email code**: OpenAI rejects extra
   login steps) in the dashboard, never in the ZIP. Run all 8 test cases yourself first.
7. **Review materials:** a short screen recording running the five positive cases in ChatGPT (developer mode), and
   release notes ("Initial release"). Commerce declaration: **No**.
8. Tick the policy attestations and **Submit**. Publishing after approval is your choice of timing.

**SourceFinch on OpenAI:** submit after the invite question is settled. OpenAI handles limited-access B2B case by case,
and reviewers need the test account to work, which it will. Expect them to ask about invite-only access.

## Submission log

| Directory | Product | Date | Status |
|---|---|---|---|
| Anthropic: MCP connector | Voydar | — | Ready; waits for Harry (and PR #84 live, ideally) |
| Anthropic: plugin bundle | Voydar | — | Ready (`plugins/voydar`) |
| Anthropic: MCP connector | SourceFinch | — | Ready; can go now with honest invite-only wording, or at launch |
| Anthropic: plugin bundle | SourceFinch | — | Ready (`plugins/sourcefinch`) |
| OpenAI: app/plugin | Voydar | — | ZIP ready; challenge route in PR #85 |
| OpenAI: app/plugin | SourceFinch | — | ZIP ready; challenge route in PR #85; explicit `destructiveHint` pending |
