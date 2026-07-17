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

The dial now has enforcement-grade settings beyond wording: pin `model`/`effort`
per skill (tier the execution, not just the prose), remove freedom mechanically
with `disallowed-tools`, and bind skill-scoped hooks for the narrow-bridge end —
the lowest freedom setting is no longer "exact script + stern prose" (see
skill-invocation-control, hook-recipes).
