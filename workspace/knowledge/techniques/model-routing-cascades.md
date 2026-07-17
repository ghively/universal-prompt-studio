---
slug: model-routing-cascades
name: Model routing and cascades
lever: orchestration
helps: high-volume traffic with mixed difficulty; cost/latency budgets
hurts: low-volume or uniform-difficulty traffic; tasks with no automatic "good enough" signal
---
Send each request to the cheapest model that can handle it. Two shapes: a
**router** predicts difficulty upfront; a **cascade** tries cheap first and
escalates on a failed check — schema violation, low confidence, verifier
rejection. Draft-cheap-then-verify-strong is the same pattern with the strong
model as checker. Production results cluster around ~2x cost reduction at ~95%
of frontier quality.

**Watch the escalation rate**: persistently above ~50% means the cheap tier is
wrong for the job; below ~5% means the check is too lax or the cascade is
unnecessary. Precondition: an automatic per-response quality signal. Prompts do
not port across tiers — each tier gets its own card-tuned prompt (see
small-model-prompting, cross-model-portability).
