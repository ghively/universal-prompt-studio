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

Runtime notes: reference bundled scripts via `${CLAUDE_SKILL_DIR}` (bare relative
paths break at personal/plugin install locations); live state the task always
needs can be spliced in before the model even reads the skill (see
skill-arguments-and-dynamic-context); and the API skill sandbox has no network
and no runtime package installs — declare dependencies in the instructions and
the spec's `compatibility` field.
