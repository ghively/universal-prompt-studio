---
slug: self-correction-chain
name: Self-correction chain
lever: orchestration
helps: high-stakes single deliverables; quality ceilings a single call can't reach
hurts: latency/cost-sensitive paths (3 calls); tasks where the critique criteria can't be stated
---
The most durable chaining pattern: **draft → critique → refine**, as separate
calls — with the field's most-replicated finding as the gate: **intrinsic
self-correction (no external signal) rarely helps and can flip right answers
wrong**. Reliable gains come when the critique is grounded in CHECKABLE external
feedback — run the tests, execute the code, validate the schema, check against
retrieved sources. Call 2 reviews against explicit checkable criteria; call 3
applies. The separate-call benefits that are certain: logging, inspection,
branching; use a DIFFERENT-MODEL critic to decorrelate errors, cheap and
consistently helpful.

Modern models handle most multistep reasoning internally — reach for explicit
chains when you need the intermediate artifacts, not to compensate for the model.

**Stopping rule**: gains concentrate in round one; further rounds are
flat-to-negative and induce sycophantic flip-flopping. One round by default;
stop when the critique passes.
