---
slug: decomposition
name: Decomposition
lever: reasoning
helps: tasks with 3+ genuinely distinct subtasks; long outputs with required sections
hurts: simple tasks (ritual numbering adds noise); reasoning models doing their own planning
---
Break the task into an explicit ordered list of steps or required output sections,
each with its own completion criterion. One numbered list, not nested trees.

**Decompose the DELIVERABLE, not the reasoning**: enumerating output sections and
obligations helps on every model; enumerating *thinking steps* is the part that
harms reasoning models (see reasoning-model-etiquette). When subtasks are
independent, escalate from one prompt to fan-out (subagents/multiple calls) —
reasoning models handle intra-task planning themselves.

**Mechanism**: models complete enumerated obligations far more reliably than implied
ones; a numbered list is a checklist the model audits itself against. Literalism
cuts both ways on current models: an instruction scoped to item 1 will NOT be
generalized to items 2-5 unless you state the scope.
