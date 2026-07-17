---
slug: deterministic-check-first
name: Deterministic checks first
lever: verification
helps: any output with checkable structure — schemas, code, citations, lengths, tool transcripts
hurts: nothing — but pure quality judgments still need a judge or human
---
Order verification layers by cost and reliability: **code checks → LLM judge →
human**. Anything that can be a schema validation, regex, unit test, string
match, or transcript assertion should be one: answer field present, cited quote
appears verbatim in source, tool X called at most N times, length under cap.
Code checks are near-free, reproducible, and never drift — an LLM judge is none
of those three. Reserve the judge for what code cannot grade; reserve humans
for calibrating the judge.

Corollary at authoring time: write output contracts to be checkable —
sentinels, extractable answer fields, quoted evidence. Verification cost is
fixed the moment the contract is written, not when the eval is built.
