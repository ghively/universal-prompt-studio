---
slug: skill-anatomy
name: Skill anatomy
lever: skill-authoring
helps: authoring any SKILL.md agent skill
hurts: n/a — these are hard format rules
---
A skill is a directory: `SKILL.md` (YAML frontmatter + markdown body) plus optional
bundled files. Hard frontmatter rules: `name` ≤ 64 chars, lowercase letters/numbers/
hyphens only, no reserved words ("anthropic", "claude"); `description` non-empty,
≤ 1,024 chars, no XML tags. **Body under 500 lines** — split into bundled files
beyond that.

Naming: prefer gerund form describing the activity (`processing-pdfs`,
`analyzing-spreadsheets`); never vague (`helper`, `utils`, `tools`).

Layout convention: instructions in SKILL.md; deep material in sibling files
(`reference.md`, `examples.md`); executables in `scripts/`. Unix paths always —
`scripts/helper.py`, never backslashes.
