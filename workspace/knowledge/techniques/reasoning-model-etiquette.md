---
slug: reasoning-model-etiquette
name: Reasoning-model etiquette
lever: reasoning
helps: targets whose card says cot_prompting is harmful (thinking/reasoning models)
hurts: n/a — this card is about what NOT to add
---
For models with native reasoning (extended thinking, R1-style), do **not** write
"think step by step", "reason carefully", or few-shot reasoning chains. Set the
thinking budget via the API and keep the prompt short and direct.

**Mechanism**: these models already allocate reasoning; in-prompt CoT duplicates it,
bloats the trace, and measurably degrades some tasks. The prompt's job is a crisp
task statement and a firm output contract — the model handles the middle.
