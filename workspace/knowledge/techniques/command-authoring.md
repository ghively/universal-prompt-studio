---
slug: command-authoring
name: Slash-command authoring
lever: harness-artifacts
helps: writing slash commands (Claude Code) and reusable parameterized prompts in any harness
hurts: n/a
---
A command is a parameterized prompt invoked deliberately — the opposite trigger
model from a skill (which self-selects by description). Use a command when the
*human* decides the moment; a skill when the *task* should summon it.

Rules: name it as the verb phrase users will type (`/review-pr`, `/write-tests`);
declare arguments explicitly and use them visibly in the body; keep the body a
complete brief (the command may fire in any conversation state — assume nothing);
end with the expected output shape. Commands are artifacts: version them, and
smoke-check them like any prompt.
