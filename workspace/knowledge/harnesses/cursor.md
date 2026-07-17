---
slug: cursor
name: Cursor
memory_files: [.cursor/rules/*.mdc (glob-scoped, YAML frontmatter), AGENTS.md (also honored)]
artifact_kinds: [MDC rule files]
---
**What's distinct**: rules are **scoped by glob and activation mode** — a rule can
apply always, on matching files, when the agent judges it relevant (via its
description), or only when manually invoked. That's finer-grained than any
always-loaded memory file.

**Prompting implications**:
- Scope aggressively: a rule about React components should glob `*.tsx`, not tax
  every request. Scoped rules are Cursor's answer to context-window pollution.
- `description`-triggered rules behave like skill descriptions — same
  what-plus-when writing discipline (skill-description-triggering applies).
- Keep individual rules short and single-topic; many small scoped rules beat one
  giant one.
- Cursor also reads AGENTS.md — use MDC only for what actually needs scoping.
