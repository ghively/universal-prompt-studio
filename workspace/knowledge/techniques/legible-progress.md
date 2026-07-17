---
slug: legible-progress
name: Legible progress reporting
lever: agentic
helps: long-running agents under human supervision; multi-window work; anything with approval gates
hurts: tight inner loops — per-step narration on a 200-call task is noise; calibrate frequency
---
You can't supervise what you can't read. Current models emit steerable progress
preambles (the GPT line calls them tool preambles: an upfront plan, then progress
notes between calls — and descriptive preambles measurably *improve* agentic
performance, they aren't just UX). Specify the report contract like any output
contract:
- Before the first tool call: a short plan (2-4 bullets).
- At milestones: done / found / next — and deviations flagged explicitly ("planned
  X, doing Y instead because...") so drift is visible while it's cheap.
- Distinguish **narration** (transient, for the human watching live) from the
  **decision log** (durable, why-shaped, for audits and restarts). Narration goes
  in the transcript; decisions go in the progress file, because artifacts survive
  compaction and are what both the next window and the human actually read
  (state-and-compaction).
Calibrate verbosity by supervision mode: detailed for gated/watched runs,
milestone-level for trusted overnight runs. Legible progress is also what makes
approval-gates workable — a gate is only as good as the report it decides on.
