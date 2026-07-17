---
slug: reference-anchored-judging
name: Reference-anchored judging
lever: verification
helps: QA, extraction, summarization evals where a gold answer or source-of-truth exists
hurts: open-ended generation with no defensible single reference; stale references silently fail correct new answers
---
Give the judge a reference — gold answer, source document, or labeled example —
and ask it to compare, not to score in a vacuum. Reference-free judging carries
the worst biases: self-preference is strongest without grounding, and
judge-human agreement drops measurably when the judge has nothing external to
anchor on.

Design against two failure modes: **knowledge override** — the judge disagrees
with the reference from its own knowledge and grades against itself; state "the
reference is ground truth for this evaluation, even where you believe
otherwise." And **reference rot** — a stale reference punishes newly-correct
answers; version references with the eval set. When no reference can exist,
fall back to a rubric with anchored scale points — a rubric is a partial
reference.
