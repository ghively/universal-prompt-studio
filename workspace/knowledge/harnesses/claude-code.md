---
slug: claude-code
name: Claude Code (CLI / desktop / web)
memory_files: [CLAUDE.md (managed -> user -> project -> nested; CLAUDE.local.md personal layer), .claude/rules/*.md (paths-scoped), auto memory MEMORY.md]
artifact_kinds: [CLAUDE.md, skills (SKILL.md — commands merged in), hooks, subagents (.claude/agents), MCP servers, plugins, output styles, permission rules]
---
**What it injects**: a large system prompt (tone, tool rules, safety), the CLAUDE.md
hierarchy (managed → user → project → nested-on-demand; delivered as a user message
after the system prompt), the budgeted skill listing (descriptions truncated at
1,536 chars inside ~1% of the window), tool schemas (MCP schemas deferred behind
tool search), auto-memory (first 200 lines / 25KB of MEMORY.md per project), and
compaction summaries on long sessions.

**Prompting implications**:
- Your CLAUDE.md competes with a big standing prompt — lean and calm wins twice
  over; official target is under 200 lines, with `.claude/rules/*.md` (`paths:`
  globs) as the escape valve for file-scoped rules.
- Durable behavior belongs in artifacts, not conversation: rules → CLAUDE.md,
  procedures → skills (slash commands are now the same artifact — invocation
  control is frontmatter), hard guarantees → hooks/permission rules
  (deterministic, can't be argued with), role isolation → subagents, packaged
  distribution → plugins, session-wide voice → output styles.
- Subagents run in the background by default, nest to 5 levels, and inherit
  CLAUDE.md + git status + (unless scoped) all tools — brief them accordingly
  (subagent-definition-authoring). Agent Teams (parallel sessions with shared
  task list + messaging) is experimental.
- Sessions compact: anything that must survive belongs in files
  (state-and-compaction). Checkpoints auto-snapshot before each prompt
  (/rewind restores code, conversation, or both — bash side effects excluded).
- Permission modes gate tools (default/acceptEdits/plan/auto/dontAsk/bypass) —
  write instructions assuming confirmation friction on destructive actions
  rather than instructing around it.
- The transcript is observable (JSONL on disk) — this studio's coach reads it.
- Multi-tool repos: Claude Code doesn't read AGENTS.md natively — put an
  `@AGENTS.md` import in CLAUDE.md (max 4 hops) or symlink, and keep
  tool-specific deltas only in CLAUDE.md itself.
