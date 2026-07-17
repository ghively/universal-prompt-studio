---
slug: skill-anatomy
name: Skill anatomy
lever: skill-authoring
helps: authoring any SKILL.md agent skill
hurts: n/a — but know which rules are per-platform validation vs universal
---
A skill is a directory: `SKILL.md` (YAML frontmatter + markdown body) plus
optional bundled files (`scripts/`, `references/`, `assets/`). The format rules
are platform-scoped, not universal:
- **Open spec (agentskills.io)**: `name` 1-64 chars, lowercase alnum+hyphen, no
  leading/trailing/double hyphen, must match the directory name; `description`
  1-1,024 chars. Optional: `license`, `compatibility`, `metadata`.
- **Anthropic API upload**: adds reserved-word bans ("anthropic", "claude").
- **Claude Code**: everything is optional — `name` defaults to the directory
  name, a missing description falls back to the body's first paragraph, and a
  much larger frontmatter surface exists (see skill-invocation-control).
**Body under 500 lines** — split into bundled files beyond that.

Naming: prefer gerund form describing the activity (`processing-pdfs`,
`analyzing-spreadsheets`); never vague (`helper`, `utils`, `tools`).

Layout convention: instructions in SKILL.md; deep material in sibling files
(`reference.md`, `examples.md`); executables in `scripts/`. Unix paths always —
`scripts/helper.py`, never backslashes. Keep platform-specific frontmatter out
of skills meant to travel (see skill-packaging-and-distribution).
