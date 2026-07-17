---
slug: reasoning-model-etiquette
name: Reasoning-model etiquette (effort, not CoT)
lever: reasoning
helps: any target whose card says reasoning kind is adaptive or always-on (most 2026 frontier models)
hurts: n/a — this card is about what NOT to add
---
Modern frontier models think natively. Do **not** write "think step by step" or
hand-written reasoning plans — in-prompt CoT anchors and degrades adaptive/always-on
models (provider migration guides; magnitude unpublished).

**The control is effort, and its semantics are per-provider AND per-model — read
the model card, not this paragraph.** Anthropic current gen:
`output_config.effort` = low|medium|high|**xhigh**|max, **default high** when
unset; xhigh is the documented sweet spot for coding/agentic; max overthinks.
OpenAI: `reasoning_effort` with a **per-model range** (none→xhigh on GPT-5.2;
max only on the newest line — a generic "none→max" claim produces invalid
requests on older models). `budget_tokens` has two states: hard 400 error on
Claude 4.7+/Sonnet 5/Fable 5; deprecated-but-functional on 4.6-gen.

**Split by reasoning kind**: on *adaptive* models, thinking *triggering* is
promptable both directions ("when in doubt, respond directly" / "this task
involves multistep reasoning — think before responding"). On *always-on* models
(Fable-class) triggering prompts are meaningless — effort is the only depth
control, and `thinking: disabled` is itself a 400.

**What still works in-prompt**: general depth cues ("consider the tradeoffs
thoroughly") outperform prescriptive step lists. Calibrate effort empirically —
see effort-calibration; never "move one level on vibes."
