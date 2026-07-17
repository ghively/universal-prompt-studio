---
slug: harness-delta-probing
name: Harness delta probing
lever: verification
helps: anyone running the same model through both a harness and the bare API; validating that card advice survives the harness
hurts: single-surface setups — no delta to measure
---
The same model is a different system inside a harness: the harness's standing
system prompt, injected memory files, tool schemas, and sampling defaults all
shift behavior. Card facts measured on the bare API don't automatically hold
inside Claude Code or Cursor — measure the delta instead of assuming it away.
Method:
- **Same battery, two surfaces**: run a deterministic probe set (format
  adherence, instruction retention, verbosity, refusal/abstention) once via
  bare API, once through the harness's headless mode (`claude -p`, CLI
  non-interactive modes). Diff per dimension.
- **Control the confounds one at a time**: empty memory files vs your real
  ones; default permission mode; no extra tools — then reintroduce each and
  re-diff. The point is attributing the delta (harness system prompt? your
  CLAUDE.md? tool pressure?), not just detecting it.
- **Probe the interactions that matter to you**: does your output contract
  survive the harness's own formatting instructions? Does an instruction in
  CLAUDE.md beat the same instruction in the turn? Do skills change compliance
  after invocation?
- **Re-run on harness updates**, not just model updates — harness release notes
  change the standing prompt under you (the same drift logic as
  version-pinning-and-drift, one layer up).
In this studio: the P8 probe battery measures the bare API; this card is the
method for the harness layer on top — store results beside `measured.json` and
let measurement beat card prose wherever they disagree.
