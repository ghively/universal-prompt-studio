---
slug: many-shot-prompting
name: Many-shot prompting
lever: examples
helps: classification/labeling with large labeled sets; style transfer; overriding a pretraining bias few-shot can't shift; fine-tuning-shaped problems without fine-tuning
hurts: reasoning models (anchors + floods thinking); tasks needing comprehension of ALL examples (quality drops well before the window fills); anything you can't cache
---
With long-context models, scaling from 5 examples to hundreds is a different regime,
not "more few-shot": measured gains across generative and discriminative tasks,
ability to override pretraining biases (few-shot cannot), and performance
approaching fine-tuning (Agarwal et al. 2024, NeurIPS). Model-generated rationales
(Reinforced ICL) or even inputs alone without rationales work in this regime.

**Constraints**: gains are task-shaped — labeling and summarization scale with
shots; translation and reasoning often plateau. Tasks requiring integration across
all examples degrade at ~16k tokens on many models even with a 1M window. Put the
example block in the stable cached prefix or the economics collapse.

**Try when**: you have >50 labeled examples, a receipt showing 5-shot
underperforms, and no fine-tuning budget. Measure at 10/50/250 shots — never
assume monotonic gains.
