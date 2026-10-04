---
name: setup-delivery
description: Deliver SourceFinch changes or snapshots to the user's own system with a signed webhook, test it, and check delivery receipts. Use for "send changes to my endpoint", "set up a webhook", "notify my app when this source changes", or debugging missed deliveries.
---

# Set up SourceFinch delivery (signed webhooks)

1. Call `list_destinations` to see what already exists. Don't create a duplicate for the same URL.
2. Ask for the receiving URL (HTTPS), a name, which sources (all, or specific `source_ids`), and the mode: `changes` (only what changed, the usual choice) or `snapshot` (all current records after each run).
3. Call `configure_delivery` with `action: "create"`. The response contains the signing secret **once**. Tell the user to store it now (for example in their secret manager or `.env`, never in git), because it can't be shown again.
4. Show how to verify the signature in their stack if they ask, using the headers documented at https://sourcefinch.com/docs/api. Deliveries carry an idempotency key, so the receiver should deduplicate on it.
5. Send a test with `configure_delivery` and `action: "test"`, then check `list_deliveries` for the receipt and response code.
6. To pause, resume or rename later, use `action: "update"`. To resend a delivery the user's server missed, use `redeliver` (same payload and idempotency key).

Only signed webhooks are available today; destinations that need stored customer credentials (Sheets, Slack, databases, S3) aren't offered yet. Don't promise them.
