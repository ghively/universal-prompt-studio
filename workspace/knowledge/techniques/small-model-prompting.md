---
slug: small-model-prompting
name: Small-model prompting
lever: model-fit
helps: local and small hosted models (~≤32B, and anything heavily quantized)
hurts: frontier models — this discipline actively degrades them (see reasoning-model-etiquette)
---
Much frontier prompting advice **inverts** below ~30B params:
- **CoT prompting helps again.** "Work through this step by step before answering"
  measurably lifts small non-reasoner models — the exact instruction that harms
  frontier adaptive thinkers.
- **Examples over descriptions**: small models follow demonstrations far better
  than abstract format rules — 2-3 examples where a frontier model needs zero.
- **One job, tight scope**: multi-constraint prompts fail quietly; split them.
- **Simpler language, shorter prompts**: long context degrades small models faster.
- **Lower temperature** (0.3-0.6) for anything factual/structured.
- **Always validate + retry structured output** — schema discipline is the first
  casualty of scale-down and quantization.

Local caveat: before judging a local model's quality, verify the chat template
(llama.cpp especially) — most "this model is bad" verdicts are template bugs.
**Toggle lore is generation-specific**: Qwen3 honors in-prompt `/think`
`/no_think`; Qwen3.5 REMOVED the soft switch (runtime params only); R1-distills
take no system prompt at all. Check the model card, not folklore.
This card is why the compiler needs the model card: the same task compiles
differently for gpt-oss-20b than for Sonnet 5.
