---
slug: prompt-vs-finetune-decision
name: Prompt vs RAG vs fine-tune
lever: model-fit
helps: deciding where to spend effort when a prompted baseline plateaus
hurts: n/a — but the default answer is "keep prompting"; tuning is the last resort, not the upgrade path
---
The escalation ladder, cheapest-first — each rung only when the previous one
measurably plateaus on your eval suite:
1. **Prompt** (with examples). Modern instruction-following plus 3-5 good
   examples covers most tasks; many-shot ICL (hundreds of examples in a long
   window) matches light fine-tuning on many benchmarks with zero training —
   try dynamic-example-retrieval before any tuning conversation.
2. **RAG** when the failure is *missing or changing knowledge*. Fine-tuning is
   bad at adding facts (it teaches form far better than content, and facts
   baked into weights go stale silently); retrieval keeps knowledge current and
   attributable (native-citations).
3. **Fine-tune** when the failure is *form at scale*: style/tone consistency,
   strict output formats, tool-call dialects, domain register — or when
   cost/latency demands compressing a long standing prompt into weights
   (distilling a frontier model's behavior onto a smaller one for one narrow
   task). Prerequisites, non-negotiable: hundreds-to-thousands of quality
   examples, a frozen eval suite proving the prompted baseline's ceiling, and
   ownership of a new maintenance burden — every base-model release triggers
   re-tuning plus re-evaluation (version-pinning-and-drift, at higher stakes).

Diagnostic: read your failures. Wrong facts → RAG. Wrong style/format that
survives prompt iteration → tuning candidate. Wrong reasoning → better model or
decomposition, not tuning. Long-prompt cost → try prompt caching first; it
erases most of the economic argument for internalizing instructions.
Combinations are normal (tuned model + RAG); the mistake is starting at rung 3.
Once you're running someone's tune, fine-tuned-model-prompting applies.
