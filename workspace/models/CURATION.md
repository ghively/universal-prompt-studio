# How the curated model cards are selected (reproducible method)

The catalog (`catalog/`) is complete and mechanical — every model on the two canonical
sources. Curated cards exist only where curation adds something a catalog row can't:
prompting idiom, reasoning-control semantics, failure modes, official guidance links.
Writing a hand-card for all 344 OpenRouter models would be fabrication at scale; a
thin card pretending to be knowledge is worse than an honest catalog row.

## Selection rule (apply on every refresh)

From the live catalog, sorted by `created` descending, card:
1. **The current flagship of every major family** present on either source
   (Anthropic, OpenAI, Google, Meta, xAI, DeepSeek, Moonshot, Z.ai, Qwen, Mistral,
   NVIDIA, MiniMax, Amazon).
2. **The price/performance pick** of any family whose flagship isn't the sensible
   default (e.g. GPT-5.6 Terra beside Sol; Gemini 3.5 Flash beside 3.1 Pro).
3. **Niche coverage** — at least one card each for: cheapest capable utility model,
   cheapest reasoning model, open-weights option, coding specialist,
   logprobs-exposing model (for the token-confidence instrument), Bedrock-native
   anchor.
4. **Retirement**: a card whose same-family successor is carded gets deleted once no
   library artifact targets it (check `target_model` across artifacts first).

## Where card facts come from (provenance, in trust order)

1. `context_window`, pricing, `params` (effort/verbosity/temperature/logprobs):
   **verbatim from the catalog snapshot** — never from memory.
2. Reasoning semantics, idiom, migration warnings: **official provider prompting
   docs**, linked in `sources`.
3. Strengths/failure modes: provider descriptions + official docs; anything beyond
   that is marked `(Card thin — verify.)` inside the card rather than invented.
4. `measured.json` (probe battery) is the only source that may contradict a card —
   measurement beats curation.

**Local models — the top-3 rule.** Local coverage is category-first, not
size-first: for each VRAM tier the owner cares about (**8 GB** and **4 GB** in
full; **16 GB** and **24 GB** ranked; 12 GB and 48 GB+ as delta-notes in
LOCAL.md), card the **top 3 models per category** (general, coding, reasoning,
vision, embedding) as ranked by current community consensus + the studio's own
probe results, using the `vram_class` and `rankings:` fields
(`[{category, vram_class, rank}]` — one model may rank in several tiers).
Embedding cards carry `kind: embedding` and serve `/v1/embeddings`. Apple
Silicon is a mapping, not a tier (see LOCAL.md); cards carry an optional
`mlx_tag:`. Rankings are re-verified on refresh — local leaderboards churn
faster than hosted ones. Canonical sources: `ollama.com/library` + Hugging Face
(see LOCAL.md). Local cards mark runtime tags `(verify with ollama list)` —
tags drift.

**Card depth is a schema field, not prose.** `card_depth: thin` means catalog facts
+ provider description only; the UI badges it, the compiler is told, and every
refresh's first job is promoting thin cards to full by researching official docs.
A thin card that stays thin across two refreshes gets demoted to catalog-only.

## Boundary decisions (2026-07-17 audit — record, don't relitigate)

- **Cohere** (`cohere-command-a`), **Tencent** (`tencent-hy3`), the
  **ultra-cheap utility pair** (`ling-2.6-flash`, `nex-n2-mini`), and
  **`gpt-5.4-nano`** are now carded THIN — catalog facts verbatim from the live
  API, capability unverified. Rule: thin cards are candidates, not defaults;
  each needs a probe battery + bake-off to displace an incumbent (Flash
  Lite/Haiku keep the utility niche until then), and a thin card that stays
  thin across two refreshes is demoted to catalog-only.
- **Claude Mythos 5**: listed on Bedrock, NOT carded — restricted to approved
  organizations, not routable through this studio's providers. This is a closed
  decision, not a gap; revisit only if it becomes reachable.
- **Router meta-models** (`openrouter/auto|fusion|pareto-code`) carry sentinel
  pricing (-1) in the catalog: excluded from the picker — a naive estimator
  computes negative cost and bypasses the budget cap (03-API guard).

Every card carries `last_reviewed`; the UI flags cards older than 120 days. The
2026-07-17 set: 55 cards (31 hosted + 24 local) against a 344-model catalog.
