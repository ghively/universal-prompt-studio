---
slug: small-model-prompting
name: Small-model prompting
lever: model-fit
helps: local and small hosted models (~≤30B, and anything quantized below Q4)
hurts: frontier models — this discipline actively degrades them (see reasoning-model-etiquette)
---
Much frontier prompting advice **inverts** below ~30B params:
- **CoT prompting helps again — with a floor.** "Work through this step by step
  before answering" measurably lifts current 4-9B instruct models (distilled on
  reasoning traces) — the exact instruction that harms frontier adaptive thinkers.
  But at the very small end (≲3B) and on legacy non-distilled models, chains come
  out fluent but illogical and CoT hurts again. Eval, don't assume.
- **Examples over descriptions**: small models follow demonstrations far better
  than abstract format rules — 2-3 examples where a frontier model needs zero.
  They are also far more sensitive to example *order* and label balance than
  frontier models: shuffle-test your few-shots, and watch for majority-label
  copying in classification.
- **One job, tight scope**: multi-constraint prompts fail quietly; split them.
- **Simpler language, shorter prompts**: long context degrades small models faster.
- **Sampling is part of the prompt**: lower temperature (0.3-0.6) for anything
  factual/structured; small models are where greedy-decoding repetition loops,
  `min_p`, and repetition penalties actually matter — a looping model is often a
  sampling bug, not a model verdict.
- **Constrained decoding is the structured-output fix**, not retry loops. Every
  local runtime this card targets supports it (llama.cpp grammars/GBNF, Ollama
  structured outputs, vLLM guided decoding): invalid JSON becomes *impossible*
  rather than caught. Validate + retry is the fallback for runtimes without it.

**Quantization, scoped**: Q4 is near-lossless for most tasks — schema discipline
does NOT collapse at sane quants. Degradation concentrates at ≤3-bit (Q3_K/Q2_K)
and hits math, code, and strict formatting first; perplexity is a poor proxy for
that damage. Rule: a smaller model at Q4 beats a bigger one at Q2.

Local caveat: before judging a local model's quality, verify the chat template
(llama.cpp especially) — most "this model is bad" verdicts are template bugs.
How: print the rendered prompt and compare against a known-good harness; the
symptoms are role bleed-through, stop-token leakage in output, and mid-turn EOS.
**Toggle lore is generation-specific**: Qwen3 honors in-prompt `/think`
`/no_think`; Qwen3.5 REMOVED the soft switch (runtime params only); R1-distills
take no system prompt at all. Check the model card, not folklore.
This card is why the compiler needs the model card: the same task compiles
differently for gpt-oss-20b than for Sonnet 5.
