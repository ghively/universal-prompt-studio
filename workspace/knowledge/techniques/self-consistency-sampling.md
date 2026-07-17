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
