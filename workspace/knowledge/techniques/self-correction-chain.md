---
slug: self-correction-chain
name: Self-correction chain
lever: orchestration
helps: high-stakes single deliverables; quality ceilings a single call can't reach
hurts: latency/cost-sensitive paths (3 calls); tasks where the critique criteria can't be stated
---
The most durable chaining pattern: **draft → critique → refine**, as separate calls.
Call 2 reviews the draft against explicit criteria (never "make it better" — name
the criteria); call 3 applies the critique. Separate calls beat one-call
self-checking when stakes are high because the critic reads fresh, without
generation momentum, and you can log/inspect/branch between steps.

Modern models handle most multistep reasoning internally — reach for explicit
chains when you need the intermediate artifacts, not to compensate for the model.
