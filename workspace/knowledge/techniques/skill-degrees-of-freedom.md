---
slug: skill-degrees-of-freedom
name: Degrees of freedom
lever: skill-authoring
helps: deciding how prescriptive each part of a skill should be
hurts: n/a — it's a calibration, not an addition
---
Match specificity to fragility. **High freedom** (heuristic text instructions) when
many approaches are valid and context should decide — e.g. code review guidance.
**Medium freedom** (templates, parameterized patterns) when a preferred shape
exists but variation is fine. **Low freedom** (exact scripts, "run exactly this,
do not modify") when operations are fragile and consistency is critical — e.g.
migrations.

The analogy from the official guidance: a narrow bridge with cliffs needs exact
guardrails; an open field needs a direction. Most bad skills err prescriptive-
everywhere (brittle, verbose) or vague-everywhere (unreliable exactly where it
mattered). Walk each section and ask: what happens if the model improvises here?
