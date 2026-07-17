---
slug: batch-processing
name: Batch processing
lever: orchestration
helps: latency-tolerant volume — evals, backfills, enrichment, nightly jobs, optimizer rollouts
hurts: anything interactive; jobs whose items depend on each other's outputs
---
Both major providers price async batch at **50% off** with a 24-hour SLA
(usually far faster), stacking with prompt caching — the largest standing
discount in the API. Consequence: classify every workload latency-sensitive or
latency-tolerant, and default the tolerant half to batch. Eval suites and
optimization rollouts — this studio's own dominant spend — are the canonical
batch workloads.

**Patterns**: idempotent per-request IDs so partial-failure reruns are safe;
poll-and-collect with per-item error handling (a batch is not all-or-nothing);
chunk mega-jobs under per-batch limits. What batch buys: a permanent 2x eval
budget for free.
