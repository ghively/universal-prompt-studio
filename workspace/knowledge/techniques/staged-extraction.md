---
slug: staged-extraction
name: Staged extraction
lever: orchestration
helps: high-volume document-to-data pipelines; extraction audited at field level
hurts: one-off extractions; pipelines with no schema to validate against
---
Extract → normalize → validate, as separate stages. **Extract**: structured
output pulls fields verbatim, an evidence/quote field per claim, explicit nulls
for absent values — never guessed defaults. **Normalize**: code, not a model —
dates, units, enum mapping are deterministic, testable, free. **Validate**:
schema plus business rules (cross-field consistency, quote-appears-in-source);
failures route to a repair call quoting the specific violation, or a human
queue — never silently through.

The design point: every model output passes through something that is not a
model before it enters your data. One call doing all three jobs hides which
stage failed and makes accuracy unauditable.
