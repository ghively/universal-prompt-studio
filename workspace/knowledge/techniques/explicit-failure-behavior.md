---
slug: explicit-failure-behavior
name: Explicit failure behavior
lever: grounding
helps: any task where input can be ambiguous, incomplete, or out of scope
hurts: forced-choice tasks; uncalibrated sentinels cause false abstention on answerable inputs (abstention inflation)
---
Tell the model exactly what to do when it cannot do the task well: a sentinel value
("write UNCLEAR"), a refusal format, or a clarifying-question protocol. Guessing must
be explicitly forbidden AND given an alternative.

**Mechanism**: without a defined failure path, models guess — confidently. A named
escape hatch converts silent wrong answers into visible, checkable signals.

**Calibrate the escape hatch — it is not free.** Measured 2025 result ("abstention
inflation"): merely *offering* a sentinel triggers false abstentions on answerable
questions — the extra option itself distorts behavior. Always pair the out with
counter-pressure ("if the answer is derivable from the input, you must answer") and
measure both failure directions, not just hallucinations caught.

**Match the hatch to the failure class.** Unanswerable inputs come in kinds —
unknown answer, underspecified ask, false premise, subjective question, stale fact —
and they need different behaviors: a false premise needs premise-challenge, not a
sentinel; an underspecified ask needs a clarifying question (see
ambiguity-surfacing); graded uncertainty belongs to confidence-elicitation. Deeper
calibration and the eval-side rules live in abstention-calibration.
