---
slug: output-style-authoring
name: Output-style authoring
lever: harness-artifacts
helps: writing output styles — system-prompt-level persona/format overrides for a whole harness session
hurts: per-task behavior (use a skill) or project facts (use CLAUDE.md)
---
An output style edits the harness's system prompt itself: markdown file with
frontmatter, applied every turn of every session that selects it. It is the
bluntest artifact — use it only for session-wide *voice and format*, never for
knowledge or workflows.
- **The one decision that matters**: `keep-coding-instructions`. `true` keeps the
  harness's engineering system prompt and appends yours (changing how it
  communicates while still coding); `false`/omitted **removes** the built-in
  software-engineering instructions — scoping, verification, comment discipline
  all gone. Dropping them for a tone tweak is the classic authoring bug.
- Body rules are ordinary system-prompt rules: calm imperatives, stated once —
  the harness already re-reminds the model to follow the style.
- Placement ladder: output style (every turn, whole session) → CLAUDE.md (project
  facts) → skill (on-demand workflow) → subagent (scoped worker). Choose the
  narrowest artifact that produces the behavior; styles cost input tokens on
  every request.
Statusline scripts are configuration, not a prompt surface — nothing there for
this library.
