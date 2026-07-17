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
