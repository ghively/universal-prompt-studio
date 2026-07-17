---
slug: lean-prompting
name: Lean prompting
lever: clarity
helps: any prompt over ~200 words, migrated prompts carrying legacy scaffolding, agent system prompts
hurts: nothing — but don't confuse lean with vague; specificity stays
---
Elaborate scaffolding now *costs* quality. OpenAI's own testing on the GPT-5.6 line:
stripping repeated instructions from system prompts raised eval scores 10–15% while
cutting tokens 41–66%. Anthropic's guidance points the same way — current models
overtrigger on redundant emphasis.

**Practice**: state each rule exactly once, in its best location. Delete restatements,
"IMPORTANT" echoes of things already said, and defensive rules for behaviors current
models no longer exhibit (laziness prompts, anti-refusal padding). The ablation prober
is the honest way to find dead weight — remove, re-run the suite, keep what didn't hurt.
