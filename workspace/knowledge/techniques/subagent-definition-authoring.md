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
  selection the same way.
- **The system prompt** (read by the subagent): a complete role brief — the
  subagent inherits nothing implicitly. Include its output contract: what it must
  return to the orchestrator (structured findings, not process narration).
- **The tool allowlist**: minimum necessary — a read-only reviewer gets no write
  tools; scoping tools is both safety and focus (fewer tools = better routing).
Model choice per agent is a real lever: judges and searchers on the cheap tier,
synthesis on the frontier tier. Test with wind-tunnel scenarios: does the
orchestrator delegate when it should (and not when it shouldn't), and does the
subagent's return format hold?
