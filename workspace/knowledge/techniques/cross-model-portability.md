---
slug: cross-model-portability
name: Cross-model prompt portability
lever: model-fit
helps: any migration or multi-target deployment (Claude ↔ GPT ↔ Gemini ↔ open/local models)
hurts: n/a — but skipping it silently breaks a few percent of traffic per migration
---
Prompts are model-fitted artifacts: measured regression suites show even
same-vendor upgrades passing only ~95-97% — at 10k queries/day that is hundreds
of silent failures (format shifts, reinterpreted ambiguity, changed reasoning
style). Migration is re-fitting, never find-and-replace. What breaks, in rough
order of frequency:
- **Reasoning controls**: effort/thinking parameters differ in name, range, and
  default per provider — a card fact, never portable.
- **Structured-output mechanism**: native structured outputs vs forced tool-use
  vs JSON mode vs (legacy) prefill; port the mechanism, not the instruction.
- **Attention profile and idiom**: some families weight system-prompt recency,
  others the top; XML vs markdown sectioning preference is per-family.
- **Instruction register**: emphasis that one model needs makes another
  overtrigger — source-model workarounds become poison on the target; strip
  them before porting, don't translate them.
- **Implicit behavior**: hosted frontier models absorb unstated conventions
  that open/local targets need spelled out (and vice versa: spelled-out rules
  overtrigger frontier targets).
- **Mechanics**: sampling defaults, stop sequences, context budget, chat
  template (local).
Checklist: freeze the eval suite → baseline on the old model → swap mechanical
params from both cards → strip source-model workarounds → re-run → diff
failures by category and fix idiom before touching content. Platform-locked
features (MCP wiring, provider tool types) never port — keep the core prompt
transferable and the platform layer separable. In this studio: a bake-off with
an un-ported prompt measures prompt fit, not model fit — port first, then race.
