---
slug: fine-tuned-model-prompting
name: Prompting fine-tuned and domain models
lever: model-fit
helps: domain-specialized instruct models (coder, medical, legal, task-tuned) and your own fine-tunes
hurts: n/a — note the prompt-vs-fine-tune *decision* is deliberately out of the studio's lane (see README known gaps)
---
Fine-tuning narrows the input distribution a model expects. The prompting job
flips from *persuading a generalist* to *matching the tune*:
- **Use the training prompt template verbatim** (model card / training
  config). Specialized models are measurably more format-sensitive than
  generalists — deviation from the tuned template costs more here than
  anywhere else, though the magnitude varies widely per model.
- **Stop re-explaining the domain.** The tuning carries the role, domain
  framing, and task conventions; verbose role/context blocks that help a
  generalist fight the tune. Prompts shrink — often to the bare input.
- **System-prompt support is not guaranteed**: some coder and distill tunes
  ignore the system slot or degrade with one (R1-distills take none at all).
  Card fact; check before blaming the model.
- **Expect a capability cliff at the domain edge**: a tuned model is worse
  than its base family outside its lane. Route out-of-scope inputs to a
  generalist instead of prompting around the cliff.
- **Your own fine-tunes**: inference prompts must mirror the training data
  format exactly — and instructions that were baked in via training data
  should be *removed* from the inference prompt, or they double-trigger.
- **Eval the tune like a new family**, not a variant: the bake-off suite and
  probe battery apply in full (see model-selection-bakeoff).
