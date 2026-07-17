---
slug: calm-imperatives
name: Calm imperatives
lever: clarity
helps: prompts written for 2023-24 models carrying "CRITICAL:", "You MUST", all-caps emphasis
hurts: nothing
---
Current frontier models are far more responsive to the system prompt than their
ancestors. Aggressive language that fixed undertriggering in old models now causes
**overtriggering**: "CRITICAL: You MUST use this tool when..." makes the model use the
tool when it shouldn't.

**Practice**: write instructions at normal conversational intensity — "Use this tool
when..." — and reserve emphasis for the one rule that genuinely outranks the others.
If a behavior still undertriggers at calm intensity, that's a signal to restructure
the instruction, not to shout it.

**Calm means unemphatic AND direct — not softened**: hedged politeness ("would you
be so kind", "if appropriate, perhaps consider") measurably degrades
instruction-following just as shouting does; blunt directness slightly outperforms
very-polite phrasing on current models [single-study, preprint]. The target is a
band: no emphasis inflation, no courtesy padding. Reasoning models literally spend
thinking tokens reconciling emphatic language.
