---
slug: fallback-chains
name: Fallback chains
lever: orchestration
helps: production paths that must survive outages, rate limits, and deprecations
hurts: prototypes; chains whose fallback path was never evaluated (an untested fallback is an outage with extra steps)
---
Tiered degradation instead of user-visible failure: primary → cross-provider
peer → cheaper model → defined floor behavior (cached answer, honest error,
reduced feature). Retries with backoff and jitter absorb transient errors;
circuit breakers stop retry storms; the chain handles the rest.

**The prompt-level work is what infra guides skip**: a prompt compiled to one
model's idioms degrades silently on the fallback model. Keep a per-tier prompt
variant compiled against each card, and run the eval suite against the fallback
path too — otherwise the worst day is also that prompt's first run. Define the
floor explicitly: "guessing is never the default" applies to systems, not just
models.
