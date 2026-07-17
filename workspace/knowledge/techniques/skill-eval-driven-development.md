---
slug: skill-eval-driven-development
name: Eval-driven skill development
lever: skill-authoring
helps: building any skill worth keeping; diagnosing a skill that "should" work
hurts: n/a
---
**Evaluations before documentation.** The official workflow: (1) run the agent on
representative tasks *without* the skill and document the specific failures; (2)
turn those failures into ~3 scenarios; (3) baseline; (4) write the *minimum*
skill content that fixes them; (5) iterate against the scenarios. This kills the
main failure mode — documenting imagined problems in beautiful detail.

Iterate with two instances: an author-model helps write and reorganize the skill;
a fresh agent-instance runs real tasks with it loaded. Watch how the agent
*navigates*: unexpected reading order means unintuitive structure; a bundled file
it never opens is dead weight or badly signaled; a file it re-reads constantly
belongs in SKILL.md. Refine from observed behavior, not assumption — and re-test
on every model tier you'll run the skill on (what's enough guidance for the
frontier model may be too little for the fast one).
