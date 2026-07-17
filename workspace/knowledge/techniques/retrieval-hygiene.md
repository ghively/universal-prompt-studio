---
slug: retrieval-hygiene
name: Retrieval hygiene
lever: context
helps: RAG pipelines; any prompt injecting searched/retrieved snippets
hurts: nothing
---
Retrieved context is only as good as its packaging:
- **Tag every snippet** with source + identifier (`<document index="3">
  <source>q3-report.pdf</source>`), so citations are possible and provenance is
  checkable.
- **License irrelevance**: "Some retrieved passages may be irrelevant — ignore
  anything that doesn't bear on the question." Without it, models strain to use
  every snippet.
- **Force grounding**: require citations to snippet indexes; claims without a
  citation are declared invalid (pairs with quote-then-answer).
- **Don't paraphrase snippets before insertion** — the model grounds better on
  original text, and paraphrase launders retrieval errors into "context."
- **Contextual retrieval is now standard**: prepend a 50-100-token LLM-generated
  context line to each chunk before embedding — Anthropic's published numbers are
  failure-rate REDUCTIONS of 35% (contextual embeddings), 49% (+contextual BM25),
  67% (+reranking). The production baseline: two-stage hybrid (BM25+dense fused
  with RRF) then cross-encoder rerank — reranking is the largest single gain.
- **Deduplicate and date**: near-duplicate retrieved chunks and semantically-
  similar-but-wrong distractors are the dominant long-context failure; dedupe/MMR
  the set, surface doc dates so the model can arbitrate conflicts, cap set size.
- **Retrieve-then-focus beats dump-everything**: the measured case for RAG even
  with 1M windows — see context-compaction for the rot data.
