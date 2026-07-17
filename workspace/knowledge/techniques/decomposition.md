---
slug: decomposition
name: Decomposition
lever: reasoning
helps: tasks with 3+ genuinely distinct subtasks; long outputs with required sections
hurts: simple tasks (ritual numbering adds noise); reasoning models doing their own planning
---
Break the task into an explicit ordered list of steps or required output sections,
each with its own completion criterion. One numbered list, not nested trees.

**Mechanism**: models complete enumerated obligations far more reliably than implied
ones; a numbered list is a checklist the model audits itself against.
