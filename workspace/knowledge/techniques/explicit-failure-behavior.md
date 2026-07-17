---
slug: explicit-failure-behavior
name: Explicit failure behavior
lever: grounding
helps: any task where input can be ambiguous, incomplete, or out of scope
hurts: nothing
---
Tell the model exactly what to do when it cannot do the task well: a sentinel value
("write UNCLEAR"), a refusal format, or a clarifying-question protocol. Guessing must
be explicitly forbidden AND given an alternative.

**Mechanism**: without a defined failure path, models guess — confidently. A named
escape hatch converts silent wrong answers into visible, checkable signals.
