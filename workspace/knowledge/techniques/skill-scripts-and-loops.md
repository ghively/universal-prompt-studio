---
slug: skill-scripts-and-loops
name: Skill scripts & feedback loops
lever: skill-authoring
helps: skills that touch fragile operations, batch changes, or quality-critical output
hurts: pure-knowledge skills with nothing to execute
---
Bundle **utility scripts** for deterministic operations — executed, not loaded:
only their output costs tokens, and a tested script beats regenerated code every
time. Say which mode you mean: "Run `analyze_form.py`" (execute) vs "See
`analyze_form.py` for the algorithm" (read). Scripts must **solve, not defer**:
handle their own error cases; no voodoo constants (every timeout/retry value gets
a justifying comment); verbose validators whose errors tell the model how to fix
the problem.

Two structural patterns that carry most of the quality:
- **Feedback loop**: make change → run validator → fix → repeat; "only proceed
  when validation passes."
- **Plan-validate-execute** for batch/destructive work: emit a plan file
  (`changes.json`) → validate it with a script → only then apply. Errors get
  caught while the plan is still reversible.
For multistep workflows, include a copyable checklist the agent checks off —
it prevents skipped validation steps.
