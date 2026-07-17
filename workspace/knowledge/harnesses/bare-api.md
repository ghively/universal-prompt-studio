---
slug: bare-api
name: Bare API (no harness)
memory_files: [none]
artifact_kinds: [system prompts, message templates]
---
**What's distinct**: nothing is injected — you assemble 100% of the context, which
is maximum control and maximum responsibility. Everything harnesses do for free
(memory, compaction, tool plumbing, permission gates) is absent until you build it.

**Prompting implications**:
- The whole placement discipline is yours: system prompt (durable, cacheable
  prefix) / documents / query-last ordering (content-placement,
  system-vs-user-placement, cache-aware-ordering all apply directly).
- Multi-turn drift is unmanaged — re-anchoring (drift-reanchoring) is your job.
- This is where this studio's artifacts run purest: a compiled prompt with a
  contract and checks IS the application layer.
