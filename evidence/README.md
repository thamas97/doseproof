# Captured runtime evidence

The engine will write JSONL records under `evidence/<run_id>/`. Retain original source identity and time, use a fresh run ID after reset, and preserve earlier runs. Runtime contents are ignored by Git by default.

Select sanitized records deliberately for a release after reviewing them for credentials and personal data. Illustrative fixtures belong in `source-examples/`. No runtime evidence has been captured yet.
