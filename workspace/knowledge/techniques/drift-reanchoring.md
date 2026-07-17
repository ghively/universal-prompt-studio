---
slug: drift-reanchoring
name: Drift re-anchoring
lever: context
helps: long multi-turn conversations and agent sessions where early constraints stop binding
hurts: short exchanges — pure overhead
---
Instructions decay with distance: fifty turns in, the persona softens and the
format slips. **Preferred channel where supported: mid-conversation system
messages** (role:"system" appended after a user turn — Opus 4.8, no beta): they
carry operator authority, preserve the cached prefix, and can't be spoofed by
injected content. Fallback for models that 400 on that: inject a compact
restatement into a user turn. Prefill re-anchoring is gone either way. Re-anchor
at intervals, before high-stakes turns, and **especially after compaction** —
that's when constraints are most likely to have been summarized away.

In agent harnesses, anchor in artifacts instead of transcript: a rules file the
agent re-reads (CLAUDE.md), state files, or hydration through a tool. Transcript
memory fades; filesystem memory doesn't.
