---
slug: uncertainty-routing
name: Uncertainty routing
lever: verification
helps: high-volume pipelines on a cheap default model; mixed-difficulty traffic
hurts: uniform-difficulty workloads; routing on an uncalibrated signal (you route noise)
---
Cascade instead of choosing one model: the cheap model answers everything, a
confidence gate accepts or escalates to a stronger model or a human. Most
volume stays cheap; hard cases get the expensive path.

**The gate is the whole game**: verbalized self-report is the WEAKEST routing
signal — agreement-across-samples, logprob/perplexity, or a small trained probe
track correctness far better. Set the threshold from measured data: pick the
accept-bucket precision you need and read the threshold off a held-out set.
Design the escalation path into the base prompt: an abstention sentinel
(CANNOT_VERIFY) is a free, well-behaved routing signal — explicit "I can't"
beats inferred "it's probably wrong." (Cost-side sibling:
model-routing-cascades.)
