---
slug: quote-then-answer
name: Quote-then-answer
lever: grounding
helps: summarization/extraction/Q&A over provided documents; any task where invented details are the failure mode
hurts: short inputs (overhead); creative tasks
---
Require the model to quote the exact supporting sentence(s) from the source before or
alongside each claim. Claims without quotes are declared invalid in the prompt.

**Mechanism**: quoting forces retrieval before generation — the model must locate
evidence, which sharply reduces confabulated specifics. It also creates a free
machine check (quote_support).

**After**: "For every fact you extract, include the exact sentence from the
transcript that supports it in supporting_quotes."
