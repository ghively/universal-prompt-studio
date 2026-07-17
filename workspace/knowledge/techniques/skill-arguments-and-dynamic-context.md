---
slug: skill-arguments-and-dynamic-context
name: Skill arguments & dynamic context
lever: skill-authoring
helps: skills that operate on a target (issue, PR, file) or need live state
hurts: pure knowledge skills with nothing to parameterize
---
Skills are parameterized prompts: `$ARGUMENTS` (full string), `$0`/`$1`
(indexed, shell-quoted, 0-based), named args declared via `arguments:`
frontmatter, `argument-hint` for autocomplete. Unmatched indexed placeholders
stay literal — write bodies that degrade gracefully when invoked bare.

Dynamic context injection is a third mode beside execute-and-read: !`git diff
HEAD` runs *before the model sees anything* and splices its output into the
body, so the skill arrives pre-grounded in live state instead of spending a
turn fetching it (```! fenced blocks for multi-line). Use it for state the
task always needs (diff, PR comments, tool versions); keep model-side tool
calls for state that's conditional — injection burns tokens even when
irrelevant. Org policy can disable it (disableSkillShellExecution), so the
body should still make sense if a placeholder reads "[disabled by policy]".

Path robustness: reference bundled scripts via ${CLAUDE_SKILL_DIR} and project
files via ${CLAUDE_PROJECT_DIR} — bare relative paths break when the skill is
installed at personal or plugin level. ${CLAUDE_SESSION_ID} and
${CLAUDE_EFFORT} enable session-scoped logging and effort-adaptive text.
