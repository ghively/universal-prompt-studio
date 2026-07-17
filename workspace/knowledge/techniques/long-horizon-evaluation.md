---
slug: long-horizon-evaluation
name: Long-horizon agent evaluation
lever: verification
helps: measuring agents whose tasks span hours, many tool calls, or multiple context windows
hurts: single-call prompts — the standard suite machinery already covers them
---
Short-task pass rates do not predict long-horizon reliability — failure
compounds per step, so a 99%-per-step agent dies on hour-long chains. Measure
differently:
- **Time-horizon framing**: characterize an agent by the task *length* it
  completes at 50% success, not by pass@1 on short tasks — the metric that
  actually tracks capability growth.
- **Milestone grading, not terminal-state grading**: score checkpoints (dense
  credit) so you learn *where* runs die — a 0/1 end-state grade hides whether
  the agent failed at step 3 or step 47. Structure tasks with checkable
  intermediate artifacts (the state files from state-and-compaction double as
  grading surfaces).
- **Put window resets inside the eval**: an agent that can't resume from its
  own handoff files fails in production and passes every single-window test.
  At least one suite case must span a forced compaction or restart.
- **Grade the exits**: classify failures by taxonomy — loop, drift from goal,
  false "done" (claimed without verification), premature give-up — each maps to
  a different fix (termination-conditions, drift-reanchoring,
  verification-first, agentic-persistence).
- **Track cost per completion** alongside success: a run that succeeds at 3x
  token budget is a different result from one that succeeds on budget.
Keep the long-horizon suite tiny (2-5 real tasks) and re-run it on model
changes — it is the expensive tier of golden-set-regression, not a daily gate.
