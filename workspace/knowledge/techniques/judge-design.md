---
slug: judge-design
name: Judge design
lever: verification
helps: LLM-as-judge prompts: rubric graders, pairwise comparisons, compliance scoring
hurts: nothing — but never trust an uncalibrated judge
---
Five documented biases: **position, verbosity, self-preference, format, calibration
drift**. Design against each:
- **Temp 0 always** — test-retest agreement falls from >95% (temp 0) to ~70% (temp 1).
- **Pairwise: swap positions and count only consistent verdicts** (not just majority).
- **Anti-length rubric line** ("do not reward length or formatting") + one criterion
  per judge, anchored scale (define 0 / 0.5 / 1.0, ideally with mini-examples).
- **Reason before score** in the output schema.
- **Never judge with the generator's own model family** (self-preference).
- **Calibrate against human anchors**; >20–25% divergence → fix the rubric wording.

**2026 correction on panels**: juries-of-judges are NOT automatically better. Judge
errors are highly correlated (models agree on wrong answers ~60% on some
benchmarks), so majority voting can dilute your best judge below its solo accuracy.
Use one strong calibrated judge by default; use a panel only if you've measured
that your judges' errors actually decorrelate — and then aggregate robustly, not
by arithmetic vote. Reliability is benchmark-specific: re-validate any judge you
move to a new domain.
