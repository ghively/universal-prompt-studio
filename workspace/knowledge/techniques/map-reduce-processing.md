---
slug: map-reduce-processing
name: Map-reduce processing
lever: orchestration
helps: documents/corpora past effective context; per-item work over many inputs
hurts: tasks needing cross-chunk reasoning (contradictions, running themes); inputs that fit one focused call
---
Split → map (one focused prompt per chunk, in parallel) → reduce (aggregate).
Context rot makes this worthwhile well before the window limit: many focused
calls beat one 100k+ dump.

**Rules**: chunk on semantic boundaries with doc metadata attached to every
chunk (the map call sees no siblings); map output is a strict schema so reduce
receives uniform inputs; reduce hierarchically when map outputs exceed budget.
The failure mode is losing GLOBAL information — sums, contradictions, entities
spanning chunks; anything global needs a metadata pre-pass or a sequential
refine variant. Map calls are batch-shaped — run them through the batch API
(see batch-processing).
