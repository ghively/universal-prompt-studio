---
slug: abstention-calibration
name: Abstention calibration
lever: grounding
helps: factual Q&A and agents where a wrong answer costs more than "I don't know"; anyone writing their own evals
hurts: forced-choice tasks (sentinel options depress accuracy on answerable items); pipelines with no IDK handling downstream
---
Prompted abstention fights the model's training objective, so calibrate it — don't
just enable it:
- **Root cause is grading**: models are optimized under 0/1 scoring where a guess
  strictly dominates "I don't know" (OpenAI, "Why Language Models Hallucinate,"
  2025; Nature 2026). A prompt-level out improves behavior but cannot cure it —
  AbstentionBench finds system prompts boost abstention without fixing uncertainty
  reasoning, and neither does scale.
- **Reasoning degrades abstention** (~24% average drop from reasoning fine-tuning,
  even in-domain). Higher effort can mean more confident answers to unanswerable
  questions; re-test abstention whenever effort level or model changes.
- **Abstention inflation is real**: an uncalibrated sentinel triggers false
  abstention on answerable items — the extra option itself, not genuine uncertainty,
  does it. Always pair the out with counter-pressure ("if the answer is derivable
  from the input, answer") and measure BOTH failure directions.
- **Fix your own scoreboard**: score IDK above confident-wrong in every eval you
  run, or your suite re-teaches the guessing incentive the prompt is fighting.
- **Test the full unanswerable taxonomy**: unknown answers, underspecification,
  false premises, subjective questions, stale facts. False premises need
  premise-challenge behavior, not a sentinel.

**Mechanism**: hallucinate-vs-abstain is a decision threshold, not a knowledge gap.
Prompting moves the threshold; only measurement tells you which direction it moved.
