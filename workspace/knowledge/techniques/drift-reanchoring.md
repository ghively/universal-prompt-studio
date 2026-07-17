---
slug: drift-reanchoring
name: Drift re-anchoring
lever: context
helps: long multi-turn conversations and agent sessions where early constraints stop binding
hurts: short exchanges — pure overhead
---
Instructions decay with distance: fifty turns in, the persona softens and the
format slips. Re-anchor by **injecting a compact restatement of the binding
constraints into a user turn** at intervals or before high-stakes turns ("Reminder
of standing rules: …"). On current Claude, this must be user-turn injection —
assistant-prefill re-anchoring is gone.

In agent harnesses, anchor in artifacts instead of transcript: a rules file the
agent re-reads (CLAUDE.md), state files, or hydration through a tool. Transcript
memory fades; filesystem memory doesn't.
