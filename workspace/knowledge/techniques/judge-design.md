---
slug: judge-design
name: Judge design
lever: verification
helps: LLM-as-judge prompts: rubric graders, pairwise comparisons, compliance scoring
hurts: nothing — but never trust an uncalibrated judge
---
Judges are prompts with known failure modes; design against them explicitly:
- **One criterion per judge.** Multi-criteria scores collapse into vibes.
- **Anchor the scale**: define what 0, 0.5, and 1.0 look like, ideally with a short
  example of each. "Score 0-1" alone drifts lenient.
- **Position bias is real** in pairwise judging: always evaluate both orders and
  aggregate; never single-order.
- **Reason before score**: force a one-sentence justification field before the
  numeric field in the schema — scores anchored on stated reasons are more stable.
- **Calibrate against human anchors**: a handful of hand-labeled outputs; if judge
  and owner disagree, fix the criterion wording, not the labels.
