---
slug: command-authoring
name: Slash-command authoring
lever: harness-artifacts
helps: writing deliberately-invoked parameterized prompts (/commands) in any harness
hurts: n/a
---
In Claude Code, commands have **merged into skills**: `.claude/commands/deploy.md`
and `.claude/skills/deploy/SKILL.md` both create `/deploy` and take the same
frontmatter. "Command vs skill" is no longer a choice between artifact types — it
is an invocation-control setting on one artifact (`disable-model-invocation: true`
for human-only, `user-invocable: false` for model-only; the default is both). The
authoring rules therefore live in the skill-authoring lever:
skill-invocation-control (who fires it, with what permissions),
skill-arguments-and-dynamic-context ($ARGUMENTS, live-state injection),
skill-anatomy and skill-description-triggering (body and listing).

What survives as command-specific advice: name it as the verb phrase users will
type (`/review-pr`, `/write-tests`); keep the body a complete brief (a command may
fire in any conversation state — assume nothing); end with the expected output
shape; version it and smoke-check it like any prompt artifact.
