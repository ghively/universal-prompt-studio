---
slug: context-budget-accounting
name: Context-budget accounting
lever: context
helps: any pipeline where prompts are assembled from parts; agents; cost-sensitive products
hurts: nothing
---
You can't manage a budget you don't measure. The working numbers:
- **Count with the provider's counter, never tiktoken** (undercounts Claude by
  ~15-20%, worse on code). Counts are model-specific — new tokenizers (current
  Sonnet/Opus) produce up to ~30% more tokens; re-baseline every threshold on
  migration.
- **Read usage correctly**: on Claude, input_tokens is only the UNCACHED
  remainder — true prompt size = input_tokens + cache_creation + cache_read.
  Dashboards reading one field lie.
- **Allocate top-down**: fixed system+tools (cached), then hard caps per slot —
  retrieval gets a token ceiling (not "top-k"), history gets a trim threshold,
  and reserve output headroom (max_tokens counts thinking + response).
- **Budget effective, not advertised, context**: rot data says 25-65% of the
  window is working memory. Set triggers against that, not the 1M sticker.
- **Never show the model its own countdown** — context anxiety; track
  harness-side.
