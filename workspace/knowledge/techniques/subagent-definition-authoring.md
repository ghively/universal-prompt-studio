---
slug: subagent-definition-authoring
name: Subagent definition authoring
lever: harness-artifacts
helps: writing subagent definition files (.claude/agents, SDK agent configs, framework agent specs)
hurts: n/a
---
A subagent definition is three prompts in one file, each with its own discipline:
- **The description** (read by the orchestrator): what + when-to-delegate, third
  person, trigger terms — identical rules to skill descriptions; it competes for
  selection the same way ("use proactively" phrasing measurably increases
  automatic delegation).
- **The system prompt** (read by the subagent): a complete role brief — written
  against the real defaults. In Claude Code a custom subagent implicitly loads
  the full CLAUDE.md/memory hierarchy, a git-status snapshot, and — with `tools`
  omitted — ALL tools including MCP. What it never gets: the conversation
  history. Include the output contract (structured findings, not process
  narration); subagents now run in the background and are resumable, so contract
  the follow-up shape too.
- **The tool allowlist**: minimum-necessary requires *explicitly writing*
  `tools:` (or `disallowedTools:`) — omitting it grants everything. A read-only
  reviewer gets no write tools; scoping is both safety and focus.
Model and effort per agent are real levers (`model: haiku` for searchers,
frontier tier for synthesis — note the built-in Explore agent now inherits the
session model unless you pin it cheaper). Test with wind-tunnel scenarios: does
the orchestrator delegate when it should (and not when it shouldn't), and does
the subagent's return format hold?
