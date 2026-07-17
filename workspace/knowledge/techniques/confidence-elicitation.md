---
slug: confidence-elicitation
name: Confidence elicitation
lever: verification
helps: factual/analytical tasks consumed downstream; countering both over-hedging and false confidence
hurts: creative tasks; adds a field consumers must handle
---
Make uncertainty explicit and structured instead of tonal: add a confidence field
to the output contract ("confidence: high|medium|low, with the reason") and an
abstention path ("if you cannot verify, say CANNOT_VERIFY rather than guessing").

Two failure modes this fixes: models hedging in prose ("it might possibly...")
which poisons downstream parsing, and models stating guesses fluently.
**Calibration correction (2025-26 research)**: verbalized confidence is
systematically OVERCONFIDENT in every format — numeric outputs saturate at
0.9/1.0, and calibration is protocol/model/dataset-sensitive. Coarse labels are
an INTERFACE argument (avoid false precision downstream), not a calibration
one. If confidence gates a decision, measure it first (does it RANK correct
above incorrect? — selective-prediction AUROC), and know that
consistency-across-samples, logprobs, or trained probes track correctness
better than self-report. Self-reported confidence is signal, not truth.
