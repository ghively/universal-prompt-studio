---
slug: version-pinning-and-drift
name: Model-version pinning and drift management
lever: model-fit
helps: any production prompt targeting a hosted model
hurts: n/a — exploratory work can float on aliases
---
Model aliases float; snapshots pin. Providers retune, redeploy, and deprecate —
documented behavior drift ("prompt drift") changes output format, reasoning
style, refusal rates, and tool-call patterns with zero edits to your prompt.
Discipline:
- **Pin dated snapshots in production**; treat the bare alias as "latest,
  untested."
- **Canary the alias**: run the floating version against a held-out suite on a
  schedule; diff against the pinned baseline before adopting.
- **Tier the regression suite by cost**: deterministic checks (schema, format,
  length, sentinels) on every run; semantic/similarity checks on every prompt
  change; full judged suite on every model change.
- **Record the exact model id in every receipt** — a quality regression you
  can't attribute to prompt-change vs model-change is unactionable. (The run
  cache already keys on model id; read it back when investigating.)
- **Deprecations put migrations on a clock**: treat every forced snapshot bump
  as a mini-migration — the cross-model-portability checklist applies at
  reduced intensity, same-family.
- **Rollback is a version pin**: when drift hits production, re-pin the prior
  snapshot first, diagnose second.
Anti-pattern: "we didn't change anything and it broke" — with a floating alias,
something changed; you just didn't version it.
