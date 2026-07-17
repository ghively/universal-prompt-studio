---
slug: few-shot-examples
name: Few-shot examples
lever: examples
helps: format-sensitive or judgment-heavy tasks where "like this" beats a paragraph of rules
hurts: reasoning models on reasoning tasks (bloats thinking); simple tasks; unrealistic examples (the model imitates the mismatch)
---
**Try zero-shot first** — 2026 frontier models follow described formats far better
than their ancestors, and examples cost tokens on every call. Reach for examples when
a receipt/suite shows the described format isn't landing.

When you do: 3–5 examples, wrapped in `<example>` tags (multiple inside
`<examples>`), **relevant** (mirror the real distribution — the model copies their
character), **diverse** (cover edge cases; vary difficulty so no unintended pattern
is learnable). Examples can include `<thinking>` blocks to model a reasoning style —
current models generalize it into their own thinking.
