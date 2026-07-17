---
slug: specificity-altitude
name: Specificity altitude
lever: clarity
helps: deciding how much detail a rule needs; open-ended work on capable models; brittle if-else prompt logic
hurts: verifiable contracts — schemas, formats, and failure behavior stay fully specified regardless of altitude
---
Specificity is a dial, not a virtue. Too high (vague: "make it good") forces the
model to guess intent; too low (hardcoded if-else for every case) creates
brittleness and conflicts — and controlled constraint-count experiments show
output quality degrades as constraints pile up. The model juggles instead of
reasons.

**Practice**: write at the altitude of your best delegation to a competent new
hire — goal, boundaries, heuristics ("prefer X over Y because Z"), not every
branch. Go fully specific only where compliance is checkable: schemas, counts,
formats, failure behavior. Ask of each detail: does it guide a decision the model
would get wrong, or occupy a decision it would get right? Delete the second kind.
