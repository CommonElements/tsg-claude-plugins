---
name: export-evidence
description: Export SourceFinch records or the evidence behind a change — CSV/JSON exports, the captured page a value came from, the signed run manifest, or the offline-verifiable WACZ evidence bundle. Use when the user needs proof, an audit trail, or the data in a file.
---

# Export SourceFinch records and evidence

Pick the narrowest output that answers the request:

- **The data as a file:** `export_run` with the run id and a format (`csv`, `json` or `ndjson`). Every row carries its record key, evidence id and evidence link; JSON and NDJSON start with a provenance header. Save it into the user's project if they want a file, and tell them the path.
- **Where one value came from:** take the record's evidence id (from `get_records` or `get_changes`) and call `get_evidence` with it. Use `include_content: true` to read the captured text; SourceFinch checks it against its SHA-256 before returning it. Quote the relevant passage and the capture time and URL.
- **Proof a third party can check:** `get_run_manifest` returns the signed manifest (Ed25519), its SHA-256 and the RFC 3161 timestamp anchor. `export_evidence_bundle` returns the WACZ bundle (captures, manifest, timestamp token, Merkle proof) up to 5 MB, or the REST download path for larger bundles. It verifies offline with SourceFinch's open verifier.

Always state the usage rights attached to the run when you hand over data, and say they are documented provenance at capture time, not legal advice.
