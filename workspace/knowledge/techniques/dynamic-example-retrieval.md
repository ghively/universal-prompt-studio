---
slug: dynamic-example-retrieval
name: Dynamic example retrieval
lever: examples
helps: high-variance input distributions where no static 3-5 examples cover the space; extraction/classification over specialized domains
hurts: cache economics (per-query examples defeat the prompt cache); pipelines without an embedding store; tasks where a static set already saturates the suite
---
Instead of one fixed example block, retrieve the k nearest labeled examples to each
incoming query (embedding similarity is the standard baseline) and splice them in
at call time. Retrieved-per-query examples consistently beat static sets on diverse
input distributions; hybrid strategies mixing a few globally representative
examples with per-query retrieved ones beat either alone.

**Known failure modes**: retrieved sets go redundant (five near-duplicates teach
less than three diverse ones — deduplicate or MMR-select); the retriever's notion
of "similar" can misalign with what actually helps the model.

**Placement**: static instructions + fixed examples in the cached prefix; retrieved
examples after the cache breakpoint, before the query. Budget the uncached tokens
against the measured accuracy gain — this technique only exists if the suite shows
the delta.
