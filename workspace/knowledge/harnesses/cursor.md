---
slug: cursor
name: Cursor
memory_files: [.cursor/rules/*.mdc (glob-scoped, YAML frontmatter), AGENTS.md (root AND nested), .cursorrules (legacy)]
artifact_kinds: [MDC rule files, AGENTS.md, team rules, hooks (hooks.json), CLI automations]
---
**What's distinct**: rules are **scoped by glob and activation mode** — current
labels: Always Apply / Apply Intelligently (agent decides from `description`) /
Apply to Specific Files (globs) / Apply Manually (@-mention), controlled by
`.mdc` frontmatter (`description`, `globs`, `alwaysApply`; a plain `.md` in
.cursor/rules is IGNORED — no frontmatter, no rule). Precedence when merged:
Team Rules → Project Rules → User Rules.

**What changed that prompt authors must know**:
- **Memories was REMOVED** (v2.1) — rules and AGENTS.md are the only persistence;
  exported memories were converted to rules. Any advice built on Cursor Memories
  is dead.
- **Nested AGENTS.md is the emphasized path**: drop plain AGENTS.md files in any
  subdirectory; nearest wins. `.cursorrules` is legacy, slated for deprecation.
- Team rules (dashboard, org-wide), hooks (`hooks.json` — audit/block/redact,
  team-level for cloud agents), Plan Mode, and a CI-capable CLI make Cursor a
  full harness, not just an editor. Composer is its in-house model line — model
  cards apply per target, same as anywhere.

**Prompting implications**:
- Scope aggressively: a rule about React components should glob `*.tsx`, not tax
  every request. Scoped rules are Cursor's answer to context-window pollution.
- `description`-triggered rules behave like skill descriptions — same
  what-plus-when writing discipline (skill-description-triggering applies).
- Keep individual rules short and single-topic (official guidance: under 500
  lines, split composables); many small scoped rules beat one giant one.
- Use MDC only for what actually needs scoping or activation modes; plain
  cross-tool truths belong in AGENTS.md.
