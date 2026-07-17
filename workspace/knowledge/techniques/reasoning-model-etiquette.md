---
slug: reasoning-model-etiquette
name: Reasoning-model etiquette (effort, not CoT)
lever: reasoning
helps: any target whose card says reasoning kind is adaptive or always-on (most 2026 frontier models)
hurts: n/a — this card is about what NOT to add
---
Modern frontier models think natively and adaptively. Do **not** write "think step by
step", "reason carefully", or hand-written reasoning plans — the model already
allocates thinking, and in-prompt CoT duplicates it, bloats traces, and measurably
degrades some tasks.

**The current control is `effort`** (Anthropic: low|medium|high|max via adaptive
thinking; OpenAI: reasoning_effort none→max). Manual `budget_tokens` is deprecated and
returns an error on Claude 4.7+ and Fable 5. Start at the default/medium, move one
level only when a *measured* quality gain justifies it.

**What still works in-prompt**: general depth cues ("consider the tradeoffs
thoroughly") outperform prescriptive step lists; and thinking *triggering* is
promptable ("extended thinking adds latency — when in doubt, respond directly").
