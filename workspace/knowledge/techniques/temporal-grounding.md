---
slug: temporal-grounding
name: Temporal grounding
lever: grounding
helps: anything touching current events, versions, prices, people's current roles; long-running agents
hurts: purely timeless tasks (dead tokens)
---
Models confidently backfill the period after their training cutoff — "temporal
hallucination" is its own failure class, distinct from misremembering. Controls:
- **State the current date** in the system prompt. Models do not reliably know
  "now"; a date enables as-of reasoning and cutoff arithmetic. (Every major harness
  injects today's date for exactly this reason.)
- **Draw the freshness boundary in the task**: "for anything that may have changed
  after your training data ends, search instead of answering from memory."
- **Route recency-sensitive claims to search grounding** (Gemini google_search
  grounding, OpenAI web_search, Anthropic web search / search results) — the
  provider-recommended fix for cutoff hallucination, with citations included.
- **Require as-of dates on time-sensitive facts** in the output contract — staleness
  becomes visible and machine-checkable instead of silent.
- Treat the advertised cutoff as optimistic: training data thins before the official
  date, so the final months before cutoff are already unreliable territory.

**Mechanism**: weights have no timestamp — "latest" in a model's memory means
latest-in-training, asserted in the present tense. Grounding either makes the as-of
explicit or replaces memory with retrieval.
