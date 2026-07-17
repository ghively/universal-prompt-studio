---
slug: claude-md-authoring
name: CLAUDE.md authoring
lever: skill-authoring
helps: writing project memory files (CLAUDE.md / agent definition files)
hurts: n/a
---
CLAUDE.md is loaded into **every** session of the project — the opposite economics
of a skill (which loads on demand). So: ruthless brevity. What belongs: the
commands that run/test/lint the project, non-obvious conventions the agent would
get wrong, pointers to deeper docs, standing behavioral rules (action defaults,
scope discipline). What doesn't: anything the agent can discover in 10 seconds of
reading the repo, tutorials, aspirations.

Write rules calmly and once (calm-imperatives applies doubly — this file is read
thousands of times, and shouted rules overtrigger). Motivate the non-obvious ones
(motivation-context). Treat it as a tested artifact: when the coach observes you
re-explaining something across sessions, that instruction belongs here — and a
should-follow scenario in the wind tunnel proves it actually changes behavior.
