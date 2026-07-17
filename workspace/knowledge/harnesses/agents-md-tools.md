---
slug: agents-md-tools
name: AGENTS.md-standard tools (Codex, Gemini CLI, Aider, Windsurf, Zed, Copilot)
memory_files: [AGENTS.md (nested, nearest-file-wins), .github/copilot-instructions.md (Copilot variant)]
artifact_kinds: [AGENTS.md files]
---
**The convention**: one markdown file at repo root (plus optional nested ones —
nearest file wins) read natively by the whole non-Anthropic coding-agent ecosystem.
Linux Foundation open standard, 60k+ repos. No schema: plain markdown sections
(setup commands, conventions, boundaries).

**Prompting implications**:
- Write AGENTS.md tool-agnostically: any of several different models may read it —
  cross-model truths only, no Claude- or GPT-specific idiom.
- Nested files scope instructions to subtrees — put module-specific rules next to
  the module, not in a giant root file.
- Copilot reads its own path; if you support it, generate it from AGENTS.md rather
  than maintaining two sources.
- Multi-tool teams: AGENTS.md is the source of truth; CLAUDE.md/.cursor rules carry
  only tool-specific deltas (many teams symlink or template them from AGENTS.md).
