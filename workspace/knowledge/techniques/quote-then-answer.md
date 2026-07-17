---
slug: quote-then-answer
name: Quote-then-answer
lever: grounding
helps: summarization/extraction/Q&A over provided documents; any task where invented details are the failure mode
hurts: short inputs (overhead); creative tasks
---
Require the model to quote the exact supporting sentence(s) from the source before or
alongside each claim. Claims without quotes are declared invalid in the prompt.
Anthropic specifically recommends quote-first extraction as a separate step for long
documents (>20k tokens): pull the relevant verbatim passages first, then do the task
over the extracts.

**Mechanism**: quoting forces retrieval before generation — the model must locate
evidence, which sharply reduces confabulated specifics. It also creates a free
machine check (quote_support).

**The catch**: prompt-side quotes are themselves generated text and can be invented.
The quote_support check must verify each quote appears **verbatim** in the source
(normalize whitespace, nothing else) — an unverified quote field is decoration.
Where the provider offers API-layer citations, prefer them: the pointer is enforced,
not requested (see native-citations — including the Anthropic citations-vs-
structured-outputs conflict that decides which mechanism you can use).

**After**: "For every fact you extract, include the exact sentence from the
transcript that supports it in supporting_quotes."
