---
slug: claude-code
name: Claude Code (CLI / desktop / web)
memory_files: [CLAUDE.md (project, nested dirs, ~/.claude global), CLAUDE.local.md]
artifact_kinds: [CLAUDE.md, skills (SKILL.md), slash commands, hooks, subagents (.claude/agents), MCP servers]
---
**What it injects**: a large system prompt (tone, tool rules, safety), all CLAUDE.md
layers (global → project → nested; always in context), skill name+descriptions
(bodies on trigger), tool schemas, and compaction summaries on long sessions.

**Prompting implications**:
- Your CLAUDE.md competes with a big standing prompt — lean and calm wins twice over.
- Durable behavior belongs in artifacts, not conversation: rules → CLAUDE.md,
  procedures → skills, parameterized prompts → slash commands, hard guarantees →
  hooks (deterministic, can't be argued with), role isolation → subagents.
- Sessions compact: anything that must survive belongs in files (state-and-compaction).
- Permission modes gate tools — write instructions assuming confirmation friction on
  destructive actions rather than instructing around it.
- The transcript is observable (JSONL on disk) — this studio's coach reads it.
