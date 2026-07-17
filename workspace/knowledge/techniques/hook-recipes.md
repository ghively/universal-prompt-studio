---
slug: hook-recipes
name: Hook recipes
lever: harness-artifacts
helps: implementing the standard deterministic guardrails once the hook-vs-prompt boundary decision is made
hurts: n/a
---
The five patterns that cover most invariants (Claude Code semantics; exit 2 =
blocking, stderr goes to the model; exit 0 + JSON = fine-grained control):
- **Protected paths**: `PreToolUse` on `Edit|Write` (or `Bash`), return
  `permissionDecision: "deny"` with a reason the model can act on. Denials teach;
  write the reason like an error message ("generated file — edit src/schema.ts").
- **Format-on-save**: `PostToolUse` on `Edit|Write` runs the formatter. Non-blocking
  by design (the edit already happened); pair with `additionalContext` to tell the
  model what changed.
- **Quality gate**: `Stop` hook runs the test suite; on failure return
  `decision: "block"` + reason. This replaces "ALWAYS run tests before finishing"
  prompt escalation — the loop physically cannot end until green.
- **Context injection**: `SessionStart`/`UserPromptSubmit` emit `additionalContext`
  (≤10k chars) — branch, open tickets, env state. Deterministic delivery of facts
  the model would otherwise have to be prompted to fetch.
- **Audit logging**: `PostToolUse` (matcher `.*` or `mcp__.*`) appends tool + input
  to a log. Zero model-facing output; pure observability.
Rules: hooks read JSON on stdin (`jq -r '.tool_input.command'`); choose exit codes
OR JSON, not both; scope hooks to a skill or subagent via their frontmatter when
the invariant only holds inside that artifact.
