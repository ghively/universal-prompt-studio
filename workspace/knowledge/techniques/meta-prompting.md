---
slug: meta-prompting
name: Meta-prompting
lever: orchestration
helps: prompts that matter enough to optimize; migrating prompts between models
hurts: unmeasured use — an "improved" prompt without an eval is just a longer prompt
---
Use a strong model to improve the prompt itself: critique against a rubric and the
target's model card, propose a rewrite, explain each change. Frontier models are
genuinely good at this — it's how this studio's compiler works.

**The discipline is downstream**: every generated improvement is a *candidate*,
never a replacement — it must beat the incumbent on the same checks before
promotion. Automated optimization loops (generate variants → eval → keep winners →
mutate) are the industrial version; they need a trustworthy metric before they're
anything but noise amplifiers.

**Disambiguation**: "meta-prompting" also names a conductor/expert orchestration
technique (Suzgun & Kalai 2024); this card means LLM-improves-the-prompt.
**Two hardening rules**: candidates must be evaluated ON THE TARGET MODEL — a
strong model's notion of better does not transfer down; and note this is now
productized (Anthropic Console improver, OpenAI optimizer) — the baseline to beat.
