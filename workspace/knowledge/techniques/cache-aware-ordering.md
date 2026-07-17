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
