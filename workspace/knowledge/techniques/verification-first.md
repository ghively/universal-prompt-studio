---
slug: verification-first
name: Verification-first prompting (define done before the task)
lever: reasoning
helps: agentic and long-horizon work; generation with checkable requirements
hurts: trivially-verifiable one-shot tasks (ritual overhead)
---
State the success criteria BEFORE the task, as checkable facts, not adjectives:
"Done means: tests pass, the CSV has a numeric price column per SKU, no file
outside src/ modified." Reasoning models verify well — but only against criteria
that exist. Vague bars produce noisy self-checks.

**Mechanism**: an explicit done-definition converts the model's native
self-verification loop from open-ended (overthinking risk) into targeted
checking; the same criteria feed harness-side grading (one expectation, stated
once, checked twice — this studio's house rule).

**Scaling up**: for autonomous runs, have the model build and run its own
checking harness on a cadence; fresh-context verifier subagents outperform
self-critique. Prefer platform-formalized rubric loops where offered.
