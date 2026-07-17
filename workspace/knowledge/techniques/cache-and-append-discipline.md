---
slug: cache-and-append-discipline
name: Cache & append discipline
lever: context
helps: agent loops and any high-frequency pipeline (cost + coherence)
hurts: nothing
---
The KV-cache rules that separate cheap, stable agents from expensive, flaky ones:
- **Stable prefix, always**: no timestamps or per-request values at the top of the
  system prompt. Invalidation is tiered on Anthropic (tools → system → messages):
  editing messages doesn't kill the tools+system cache; changing tool definitions
  or the model kills everything.
- **Append-only history**: never edit or reorder earlier turns mid-episode.
- **Tool surfaces: append, mask, never remove**: Anthropic's tool-search tool
  (defer_loading) APPENDS discovered schemas so the prefix survives — the current
  best mechanism; masking is the fallback; deleting tools mid-episode breaks cache
  AND confuses the model.
- **Keep errors in context**: scrubbing failed attempts removes the evidence the
  model learns from within the episode.
- **Recite the goal**: long agents drift; periodically rewriting a todo/goal
  statement near the end of context fights lost-in-the-middle.
- Several providers now expose cache keys (prompt_cache_key / conversation
  headers) — set them; cache routing is not automatic everywhere.
supersedes-note: extends cache-aware-ordering from single prompts to agent loops.

**Agent-loop cache mechanics (the invisible killers)**: a cache breakpoint looks
back at most 20 content blocks — turns emitting more tool blocks than that silently
miss; add intermediate breakpoints every ~15. Parallel identical-prefix requests
all pay full price (cache is readable only after the first response starts) — fire
one, await first token, then fan out. Pre-warm with a max_tokens:0 request. VERIFY
hits: cache_read_input_tokens stuck at 0 across repeats = a silent invalidator.
