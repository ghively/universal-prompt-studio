# Harness cards — the harness is part of the prompt

The same model behaves differently inside Claude Code, Cursor, a LangGraph app, or a
bare API call, because the harness assembles most of the context the model actually
sees: its own system prompt, memory files, tool definitions, permission gates,
history compaction. Prompting advice that ignores the harness is incomplete —
"where does this instruction go?" depends on which runtime executes it.

One card per harness: what it injects, which memory/artifact files it reads, its
tool and permission surface, and what that means for how you write instructions.
The profiler asks which harness a recurring/agentic task runs in; the compiler
receives the matching card.

The 2026 landscape in one line: every coding agent converged on repo-root markdown
memory files — **AGENTS.md is the cross-tool open standard** (nested,
nearest-file-wins; read by Codex, Cursor, Copilot, Gemini CLI, Aider, Windsurf,
Zed), **CLAUDE.md** is Claude Code's richer variant, **.cursor/rules/*.mdc** adds
glob-scoped activation. If you run multiple tools: AGENTS.md as source of truth,
per-tool overrides only where needed.

Frontmatter per card: `slug`, `name`, `memory_files`, `artifact_kinds`.
