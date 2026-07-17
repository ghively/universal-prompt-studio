---
slug: skill-invocation-control
name: Skill invocation control
lever: skill-authoring
helps: deciding who fires a skill, with what permissions, and in which context
hurts: n/a — every skill makes these choices, even by silent default
---
Skills and slash commands merged: every skill is also `/name`, and frontmatter
decides who may fire it. `disable-model-invocation: true` = human-only — use
for side effects and timing-sensitive work (deploy, commit, send-message);
it also removes the description from context entirely (zero listing cost).
`user-invocable: false` = model-only background knowledge that isn't a
meaningful command. Default = both.

Permissions ride along: `allowed-tools` pre-approves listed tools for the
invoking turn only (the grant clears on the next user message, even though the
skill content persists); `disallowed-tools` removes tools while active — e.g.
no AskUserQuestion for an autonomous loop. Consumers can gate skills with
permission rules (`Skill(deploy *)` deny) and `skillOverrides`; author as if a
gatekeeper exists.

Placement: `context: fork` runs the skill in a subagent (`agent:` picks which)
— only for bodies that are complete task briefs; a forked guidelines-skill
returns nothing useful. `paths:` globs scope auto-loading to matching files.
`model`/`effort` pin the execution tier per skill. Skill-scoped `hooks:` bind
deterministic checks to the skill's lifetime (`once: true` for one-time setup)
— the hook-vs-prompt boundary decision, made inside the skill file itself.
