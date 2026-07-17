---
slug: agents-md-tools
name: AGENTS.md-standard tools (Codex, Copilot, Windsurf, Zed, Cline, Devin, Junie...)
memory_files: [AGENTS.md (nested, nearest-file-wins), .github/copilot-instructions.md (Copilot variant)]
artifact_kinds: [AGENTS.md files]
---
**The convention**: one markdown file at repo root (plus optional nested ones —
nearest file wins) stewarded by the Agentic AI Foundation under the Linux
Foundation (since Dec 2025, alongside MCP and goose); 60k+ repos. No schema:
plain markdown sections (setup commands, conventions, style, gotchas,
do-not-touch list). Read natively by Codex, Cursor, GitHub Copilot coding agent,
Windsurf, Amp, Zed, Jules, Devin, Junie, opencode, and Cline. **Support levels
differ — two common overstatements**: Gemini CLI defaults to GEMINI.md and reads
AGENTS.md only via `context.fileName` config; Aider loads conventions only
explicitly (`/read` / `--read` / .aider.conf.yml), no auto-detection. Zed reads
it natively but `.rules`/`.cursorrules` outrank it. And **Claude Code does NOT
read AGENTS.md natively** — bridge with an `@AGENTS.md` import in CLAUDE.md or a
symlink.

**Codex specifics worth knowing**: global `~/.codex/AGENTS.md`, per-level
`AGENTS.override.md` checked first, files concatenated root-down (nearest last =
wins), 32 KiB default budget. Approval modes are now
`untrusted | on-request | never` plus sandbox `read-only | workspace-write |
danger-full-access` — the old suggest/auto-edit/full-auto trio is legacy.

**The fork lineage (mid-2026)**: Cline (4.6M installs) auto-detects AGENTS.md
plus `.clinerules/` dirs and token-cheap `/workflows`; Roo Code is dead (repo
archived May 2026 — migrate to Cline or Kilo); Kilo's CLI is an opencode fork.

**Prompting implications**:
- Write AGENTS.md tool-agnostically: any of several different models may read it —
  cross-model truths only, no Claude- or GPT-specific idiom.
- Nested files scope instructions to subtrees — put module-specific rules next to
  the module, not in a giant root file.
- Copilot reads its own path; if you support it, generate it from AGENTS.md rather
  than maintaining two sources.
- Multi-tool teams: AGENTS.md is the source of truth; CLAUDE.md/.cursor rules carry
  only tool-specific deltas (many teams symlink or template them from AGENTS.md).
