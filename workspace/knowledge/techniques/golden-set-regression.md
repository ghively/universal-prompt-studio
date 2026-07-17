---
slug: golden-set-regression
name: Golden-set regression
lever: verification
helps: any prompt that will be edited more than once; model migrations; drift detection
hurts: nothing — but an unmaintained golden set decays into false confidence
---
A prompt without a regression set is tuned on vibes. Keep a versioned golden
set — dozens to a few hundred input/expected pairs — and run it on every prompt
or model change. Rules:
- **Source from real failures**, not synthetic examples; every production
  failure becomes a case.
- **Track per-item flips**, not just the aggregate — a flat average hides one
  regression canceling one fix.
- **Hygiene**: the set you iterate against is effectively training data —
  hold out a slice you never look at while editing, and probe with paraphrase
  variants; if held-out or paraphrased performance lags, you've memorized your
  own eval.
- **Drift detection**: re-run unchanged prompts on a schedule; on managed
  models, silent updates are a real regression source (see
  version-pinning-and-drift).
