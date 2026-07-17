---
slug: system-vs-user-placement
name: System vs user placement
lever: structure
helps: any app with a system prompt; recurring tasks with a per-call payload
hurts: nothing
---
The split that works: **system** carries durable identity and rules (role, tone,
standing constraints, output conventions, tool guidance) — everything true for
every call; **user** carries this call's task and content. Current models weight
the system prompt heavily and follow it more literally than their ancestors.

**Why it matters beyond tidiness**: the system prompt is the stable cache prefix
(cost), per-call content in the user turn can't dilute standing rules (drift), and
rules in the user turn compete with data for attention. Don't restate system rules
in the user turn "to be safe" — that's the repetition that measurably hurts.
