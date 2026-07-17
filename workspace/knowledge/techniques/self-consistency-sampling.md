---
slug: self-consistency-sampling
name: Self-consistency sampling
lever: verification
helps: checkable tasks where single-run reliability is too low (extraction, math, classification edge cases)
hurts: cost/latency budgets (N× price); subjective tasks (majority vote of opinions is noise)
---
Run the same prompt N times (3–5) and take the majority answer, or generate N
candidates and select the best with a judge pass (best-of-N). Reliability rises
sharply when errors are uncorrelated.

**Rules**: only worth it when a wrong answer costs more than N runs; needs an
extractable answer to vote on (add an answer field to the output contract); and
measure — if consistency is already ~1.0, sampling buys nothing (the studio's
consistency probe tells you this before you pay for it).

**2026 update — diminishing returns**: on frontier/reasoning models the gains
shrink sharply and majority voting can DEGRADE performance on problems the model
already solves reliably (the technique was built for high-variance 2022 models).
On effort-capable models, raising effort is usually the cheaper first move.
Modern practice is confidence-weighted voting with early stopping (40-80% cost
reduction at equal accuracy), not fixed-N arithmetic vote. And a single model's
errors at high capability are increasingly SYSTEMATIC — the same correlated-error
ceiling that demoted judge panels caps voting. Bonus use: agreement-rate across
samples is one of the best cheap confidence signals (see uncertainty-routing).
