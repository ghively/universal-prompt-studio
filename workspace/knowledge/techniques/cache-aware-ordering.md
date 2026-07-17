---
slug: cache-aware-ordering
name: Cache-aware ordering
lever: context
helps: recurring prompts run many times (pipelines, daily jobs)
hurts: nothing functionally; irrelevant for one-offs
---
Keep everything that never changes (role, rules, schema, examples) as one contiguous
block at the top; inject variable content only at the bottom. Never interleave.

**Mechanism**: provider prompt caches match on exact prefixes. A stable prefix means
each recurring run pays full price only for the variable tail — often a 5-10x input
cost reduction — with zero quality tradeoff.

**Provider mechanics (2026)**: Anthropic — explicit cache_control breakpoints (max
4), reads 0.1x but WRITES 1.25x (5-min TTL) or 2x (1-hour); break-even at 2-3
requests; minimum cacheable prefix is MODEL-DEPENDENT (4096 tokens on Opus-class,
2048 on Fable 5/Sonnet 4.6, 1024 on Sonnet 4.5) — short prompts silently don't
cache at all. OpenAI — automatic ≥1024 tokens, discount 50-90% by model, set
prompt_cache_key for reliable routing, retention up to 24h. Gemini — implicit 90%
discount on 2.5+; explicit CachedContent needs ≥32k tokens and bills storage
($1/MTok/hr). "5-10x savings" is the high-repetition best case, not a default.
