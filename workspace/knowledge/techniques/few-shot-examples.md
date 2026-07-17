---
slug: few-shot-examples
name: Few-shot examples
lever: examples
helps: format-sensitive or judgment-heavy tasks where "like this" beats a paragraph of rules
hurts: reasoning models on reasoning tasks — measured ACCURACY DROPS, not just cost (R1's card recommends zero-shot; 5-shot degraded o1-class models); unrealistic examples (the model imitates the mismatch)
---
**Try zero-shot first** — 2026 frontier models follow described formats far better
than their ancestors. Reach for examples when a receipt/suite shows the described
format isn't landing. The 3–5 default remains right for the classic regime; for the
hundreds-of-examples regime on long-context models, see many-shot-prompting.

When you do: examples in `<example>` tags (multiple inside `<examples>`),
**relevant** (mirror the real distribution), **diverse** (cover edge cases), and —
critical on current models — **correct**: frontier/instruction-tuned models learn
the actual input→label mapping, so a wrong or sloppy example actively teaches the
wrong mapping (flipped labels crater performance on large models where small models
barely noticed).

**Biases with receipts**: order matters (recency bias — models over-predict the last
example's label; randomize, never sort by label) and label distribution matters
(majority-label bias — balance labels unless you're deliberately mirroring a real
skew). Order sensitivity in your suite is a signal the task is under-specified.

**Economics**: a static example block in the stable cached prefix costs ~10% of
sticker input price on re-runs — the "examples cost tokens" objection mostly applies
to dynamic per-query examples (see dynamic-example-retrieval) and to thinking-token
anchoring on reasoning models. When only structure matters, one schema-true
skeleton with placeholder content teaches format without biasing content — the
cheapest example you can buy.
