---
slug: process-vs-outcome-eval
name: Process vs outcome evaluation
lever: verification
helps: agents and multi-step pipelines; tasks where a right answer by a wrong path is a latent failure
hurts: simple single-call prompts; process rubrics can reward ritual over results
---
Outcome-only grading (final answer pass/fail) can't distinguish lucky successes,
destructive detours, and unrecoverable habits. Grade the trajectory too:
- **Deterministic transcript assertions first**: required tool called, forbidden
  action absent, budget respected, no writes outside scope.
- **Trajectory judge (agent-as-judge) second**: tool choice sensible, errors
  recovered, no fabricated observations.
- **Keep outcome as the gate and process as the diagnosis** — a process score
  that can pass a failed task inverts the incentive: agents learn the ritual.
The trajectory is data: keep it and grade it (this studio's wind tunnel does
exactly this — activation/compliance/outcome).
