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

**Scope — the field's most-replicated finding**: intrinsic self-correction FAILS
on reasoning correctness (no external signal = no reliable gains, and right
answers get flipped; models even show a blind spot — they fix an error presented
as input but not the same error in their own output). It WORKS on decomposable,
checkable constraints: format, coverage, instruction compliance — or when an
external signal (test result, retrieved doc) is injected. Use this technique for
constraint verification, not correctness verification. A fresh-context or
different-model critic outperforms same-context self-review. Measure the
fix-rate before paying double for the second call.
