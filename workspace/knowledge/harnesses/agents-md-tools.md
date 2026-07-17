---
slug: agents-md-tools
name: AGENTS.md-standard tools (Codex, Gemini CLI, Aider, Windsurf, Zed, Copilot)
memory_files: [AGENTS.md (nested, nearest-file-wins), .github/copilot-instructions.md (Copilot variant)]
artifact_kinds: [AGENTS.md files]
---
**The convention**: one markdown file at repo root (plus optional nested ones —
nearest file wins) read natively by Codex, Cursor, GitHub Copilot coding agent,
Windsurf, Amp, Aider, Gemini CLI, Zed, Jules, Devin, and Junie. Stewarded by the
Agentic AI Foundation under the Linux Foundation (since Dec 2025); 60k+ repos. No
schema: plain markdown sections (setup commands, conventions, style, gotchas,
do-not-touch list). NOTE: **Claude Code does NOT read AGENTS.md natively** — bridge
with an `@AGENTS.md` import line in CLAUDE.md or a symlink.

**Prompting implications**:
- Write AGENTS.md tool-agnostically: any of several different models may read it —
  cross-model truths only, no Claude- or GPT-specific idiom.
- Nested files scope instructions to subtrees — put module-specific rules next to
  the module, not in a giant root file.
- Copilot reads its own path; if you support it, generate it from AGENTS.md rather
  than maintaining two sources.
- Multi-tool teams: AGENTS.md is the source of truth; CLAUDE.md/.cursor rules carry
  only tool-specific deltas (many teams symlink or template them from AGENTS.md).
