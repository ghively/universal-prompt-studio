---
slug: system-vs-user-placement
name: System vs user placement
lever: structure
helps: any app with a system prompt; recurring tasks with a per-call payload
hurts: putting per-call or untrusted content in system — it destroys the cache prefix and elevates injected text to maximum authority
---
The split that works: **system** carries durable identity and rules (role, tone,
standing constraints, output conventions, tool guidance) — everything true for
every call; **user** carries this call's task and content. Current models weight
the system prompt heavily and follow it more literally than their ancestors.

**Why it matters beyond tidiness**: the system prompt is the stable cache prefix
(cost), per-call content in the user turn can't dilute standing rules (drift), and
rules in the user turn compete with data for attention. Don't restate system rules
in the user turn "to be safe" — that's the repetition that measurably hurts.

**The modern hierarchy** (see message-role-semantics): OpenAI renamed system →
developer and trains an authority chain platform > developer > user > tool, with
tool outputs and quoted data carrying no authority. The two-way system/user
mental model under-describes OpenAI-lineage targets.
