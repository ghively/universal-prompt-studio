---
slug: thinking-trace-hygiene
name: Thinking-trace hygiene (visibility, leakage, replay)
lever: reasoning
helps: anyone surfacing model output to users; multi-turn/agentic harnesses
hurts: n/a — plumbing card
---
Three facts get conflated: what the model thinks, what you can see, what the
user reads. Keep them straight per model card.

**Visibility is a parameter**: Anthropic thinking.display = summarized | omitted —
and the DEFAULT flipped to omitted on current models (streaming UIs show a long
silent pause unless you opt into summaries). Raw CoT is never returned on
frontier-class models; billing is identical either way.

**Leakage into output**: with thinking disabled, current models may write
reasoning into the visible response — leave adaptive thinking on, or instruct
"Respond only with your final answer — no exploratory reasoning or process
commentary." Long-session working shorthand also leaks into summaries; ask for a
re-grounded plain-language summary.

**Replay rules**: echo thinking blocks back unchanged when continuing on the same
model; other models drop them silently. Never edit or strip mid-conversation.
Prompting a model to reveal internal reasoning can trigger a reasoning_extraction
refusal (Fable-class) — read the summarized trace instead.
