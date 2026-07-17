---
slug: self-check-pass
name: Self-check pass
lever: verification
helps: high-stakes single calls; long outputs with many constraints
hurts: cost/latency-sensitive calls; can induce needless revision on subjective tasks
---
End the prompt with: "Before responding, verify your output against each requirement
above; fix any violation, then output only the final version." For stronger effect,
make it a second call: generate, then critique-and-fix.

**Mechanism**: generation and verification are different operations; an explicit
verification instruction makes the model re-read its own output against the rules
instead of stopping at fluent.
