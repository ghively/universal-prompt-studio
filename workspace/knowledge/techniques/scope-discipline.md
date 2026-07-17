---
slug: scope-discipline
name: Scope discipline
lever: agentic
helps: coding agents (current frontier models overengineer by default); any generation task with a defined deliverable
hurts: genuinely open-ended "go beyond the basics" requests — say that explicitly instead
---
Current top-end models add unrequested features, abstractions, defensive code, and
files. Counter with explicit minimalism: "Only make changes directly requested or
clearly necessary. No extra configurability, no docstrings on untouched code, no
error handling for impossible scenarios, no helpers for one-time operations. The
right amount of complexity is the minimum for the current task."

Two related blocks worth pairing: anti-test-gaming ("tests verify correctness, not
define the solution — implement the general logic; tell me if a test is wrong rather
than working around it") and cleanup ("remove any temporary files you created").

Scope has a second half: **action scope** — "don't take actions beyond what the
task requires" is the security-relevant discipline (least agency; see
least-privilege-tooling), not just code hygiene. And give minimalism an escape
hatch: "if you believe a broader change is needed, propose it — don't do it."
Without the hatch, the rule silently suppresses legitimate findings.
