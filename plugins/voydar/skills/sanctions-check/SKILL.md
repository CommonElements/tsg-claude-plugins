---
name: sanctions-check
description: Screen vessels against the OFAC SDN, UK, Canadian and EU sanctions lists with Voydar and produce a sourced vessel risk brief. Use for "is this ship sanctioned", "screen these IMOs", "due diligence on vessel X", "list OFAC-designated vessels".
---

# Sanctions and risk check with Voydar

1. Collect identifiers. Use IMO numbers (or MMSIs). Resolve names with `search_entities` first; screening is by identifier only.
2. Call `screen_vessels` with up to 50 identifiers. For each vessel, report active and past designations with the list, programme, dates and the source link.
3. Word the result exactly: matches are by the listed IMO or MMSI, never by name. "Not sanctioned" means **no designation carries that identifier** on those four lists. It is not a clearance, and it doesn't cover owners, cargo or other lists. Say so.
4. For a full brief on one vessel, also call `get_vessel` (ownership, inspections, emissions, other names) and `get_port_calls` (recent movements), and structure it as: identity, ownership, sanctions, inspections, emissions, recent movements, sources. Voydar's `vessel_due_diligence` prompt follows this shape.
5. To browse a whole list, use `list_sanctioned_vessels` with `ofac`, `uk`, `canada` or `eu` and page with `nextCursor`.

This is screening support, not legal advice. Tell the user to verify designations at the linked source before acting.
