---
slug: native-citations
name: Native citation features
lever: grounding
helps: RAG and document Q&A on providers with citation APIs; any output whose claims must be auditable
hurts: strict-JSON outputs on Anthropic (citations + structured outputs = 400 error); trivial single-fact lookups (overhead)
---
When the provider offers citation machinery, prefer it over prompt-side quoting — the
pointer is enforced at the API layer, not requested in prose:
- **Anthropic Citations API**: `citations: {"enabled": true}` on `document` blocks
  (plain text, PDF, custom content) or `search_result` blocks. Responses interleave
  text with citation objects; `cited_text` is extracted by the API — guaranteed to
  point into the source, impossible to confabulate — and counts toward neither output
  tokens nor subsequent input tokens. Anthropic measures up to 15% better recall than
  prompt-based quoting. Chunking sets granularity: sentence-level for text/PDF;
  custom content blocks cite as-is (use one block per RAG chunk, transcript turn, or
  bullet when sentence chunking is wrong).
- **Gemini**: `google_search` grounding returns `groundingMetadata` —
  `groundingSupports` maps answer spans (start/end index) to `groundingChunks`
  sources; the URL-context tool grounds on caller-supplied pages the same way.
- **OpenAI Responses API**: `web_search` and `file_search` return `url_citation` /
  `file_citation` annotations with character offsets into the response.

**The tradeoff to decide up front (Anthropic)**: citations and structured outputs are
mutually exclusive — cited prose or schema'd JSON, not both. If the contract needs
JSON, fall back to quote-then-answer fields inside the schema plus a verbatim
quote-verification check.

**Mechanism**: prompt-side quotes are themselves generated text and can be invented;
API-layer citations are parsed and validated against the document, converting
"please quote" from a request into a guarantee.
