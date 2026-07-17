---
slug: cache-and-append-discipline
name: Cache & append discipline
lever: context
helps: agent loops and any high-frequency pipeline (cost + coherence)
hurts: nothing
---
The KV-cache rules that separate cheap, stable agents from expensive, flaky ones:
- **Stable prefix, always**: no timestamps or per-request values at the top of the
  system prompt — one changed early token invalidates the whole cache.
- **Append-only history**: never edit or reorder earlier turns mid-episode.
- **Mask tools, don't remove them**: dynamically deleting tools mid-episode breaks
  cache AND confuses the model — constrain choice instead.
- **Keep errors in context**: scrubbing failed attempts removes the evidence the
  model learns from within the episode.
- **Recite the goal**: long agents drift; periodically rewriting a todo/goal
  statement near the end of context fights lost-in-the-middle.
- Several providers now expose cache keys (prompt_cache_key / conversation
  headers) — set them; cache routing is not automatic everywhere.
supersedes-note: extends cache-aware-ordering from single prompts to agent loops.
