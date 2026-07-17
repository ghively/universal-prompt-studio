---
slug: few-shot-examples
name: Few-shot examples
lever: examples
helps: format-sensitive or judgment-heavy tasks; anything where "like this" beats a paragraph of rules
hurts: reasoning models (bloats their thinking); simple tasks (wasted tokens); examples that don't match the real distribution (the model imitates the mismatch)
---
One to three worked input→output examples are the single strongest lever for format
and style fidelity. The model imitates examples more faithfully than it follows
descriptions.

**Mechanism**: examples collapse ambiguity that prose cannot — length, tone, edge
handling, field order — all demonstrated at once.

**Rules**: examples must be *realistic* (the model copies their character), *diverse*
(vary the difficulty), and *few* (returns diminish fast after 3).
