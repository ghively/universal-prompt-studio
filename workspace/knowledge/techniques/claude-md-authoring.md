---
slug: claude-md-authoring
name: CLAUDE.md authoring
lever: skill-authoring
helps: writing project memory files (CLAUDE.md / agent definition files)
hurts: n/a
---
CLAUDE.md is loaded into **every** session of the project — the opposite economics
of a skill (which loads on demand). Official target: **under 200 lines**. What
belongs: the commands that run/test/lint the project, non-obvious conventions the
agent would get wrong, pointers to deeper docs, standing behavioral rules (action
defaults, scope discipline). What doesn't: anything the agent can discover in 10
seconds of reading the repo, tutorials, aspirations.

Mechanics worth knowing: the file is delivered as a *user message after the
system prompt*, not inside the system prompt; block-level HTML comments are
stripped before injection (free maintainer notes); `@path` imports expand at
launch (max 4 hops — they do NOT save context); the project-root file is
re-injected after compaction but nested ones are not. Escape valves for bloat:
path-scoped rules (`.claude/rules/*.md` with `paths:` glob frontmatter) load only
when matching files are touched; `CLAUDE.local.md` is the gitignored personal
layer; AGENTS.md interop is via import or symlink, not native reading. Auto
memory (MEMORY.md) is the sibling system that absorbs "Claude keeps re-learning
X" observations across sessions.

Write rules calmly and once (calm-imperatives applies doubly — this file is read
thousands of times, and shouted rules overtrigger). Motivate the non-obvious ones
(motivation-context). Treat it as a tested artifact: when the coach observes you
re-explaining something across sessions, that instruction belongs here — and a
should-follow scenario in the wind tunnel proves it actually changes behavior.
