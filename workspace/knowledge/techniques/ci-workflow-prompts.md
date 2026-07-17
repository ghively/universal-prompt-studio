---
slug: ci-workflow-prompts
name: CI workflow prompts
lever: harness-artifacts
helps: authoring prompts that run headless in CI (claude-code-action, scheduled jobs, PR automation)
hurts: n/a
---
A CI prompt (`prompt:` in claude-code-action@v1, or any headless invocation) is a
prompt with no human in the loop — author accordingly:
- **The prompt is the whole conversation.** No clarifying questions, no retries.
  State the task, the inputs (interpolate `${{ github.event.* }}` context), the
  output contract, and explicit failure behavior ("if tests can't run, comment
  saying so and stop" — guessing in CI writes bad commits).
- **Bound it mechanically**: `claude_args: --max-turns N`, workflow timeouts, and
  the narrowest `--allowedTools` list that completes the task. Standing behavior
  goes in `--append-system-prompt` or CLAUDE.md, not repeated per-workflow.
- **Prefer invoking a versioned artifact over inline prose**: `prompt: "/code-review
  ..."` runs a repo skill — the prompt logic lives in one reviewed file instead of
  scattered YAML strings. Checkout first for repo skills; `plugins:` for packaged
  ones.
- **Two trigger modes, two prompt styles**: @claude-mention workflows respond to a
  human turn (prompt optional); event-triggered automation (schedule, PR opened)
  must carry a complete standalone brief.
- Secrets stay in GitHub Secrets; never in the prompt text — prompt files are the
  most-copied artifacts in a repo.
