---
slug: input-format-choice
name: Input format choice
lever: structure
helps: prompts that ingest tabular or record data (CSV exports, DB rows, logs, API responses)
hurts: nothing — but the accuracy-vs-token tradeoff cuts both ways
---
The serialization of ingested data changes retrieval accuracy by double digits.
Record-lookup benchmarks: markdown key-value ~61% > XML ~56% > YAML ~55% > JSON
~52% ≈ markdown table > JSONL ~45% > **CSV ~44%** > pipe-delimited. CSV loses
because nothing distinguishes headers from values; formats that label every field
win.

**The tradeoff**: markdown-KV costs ~2.7x the tokens of CSV. Small data, high
stakes → verbose labeled formats. Large data near budget → compact formats plus
retrieval, not a 100k-token labeled dump (context rot outweighs format gains).

**Practice**: never paste raw CSV for reasoning tasks — convert to a markdown
table (cheap) or key-value records (accurate). Rankings are model-specific;
measure on the target.
