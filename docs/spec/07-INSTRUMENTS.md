# 07 — Instruments (P3)

Attached to any artifact run via `POST /api/artifacts/:slug/run` with `instruments`.

## 1. Interpretation report
Render the artifact body with the provided vars → run INTERPRETATION (04-PROMPTS §6)
on the **artifact's target model** (purpose `interpretation`).
Response fragment: `{ "interpretation": { restatement, ambiguities, assumptions, conflicts, ignored, usd } }`.
UI: violet-washed section titled `How it read your prompt` with subtitle
`self-reported by the model — useful, not gospel` (12px `--tx-faint`). Lists render
only when non-empty; ambiguity readings as sub-bullets.

## 2. Consistency probe
Run the rendered prompt N times (config `consistency_samples`, default 3) on the target
model at `temperature: 1.0`, purpose `consistency` (cache **bypassed**).
Clustering (mechanical): outputs O1..On; clusters start as [O1]; for each Oi compare
against the first member of each existing cluster with SAME_ANSWER_JUDGE (§5.3, on
`cheap_model`); join the first cluster judged `same`, else start a new cluster.
Agreement score = size of largest cluster / n.
Response fragment: `{ "consistency": { samples: n, agreement: 0.67, clusters: [ { count, representative_output, difference: str|null } ], usd } }`.
UI: section `Does it hold its answer?` — big agreement figure (display face, `--pass`
if 1.0, `--human` if ≥ 0.5, `--fail` below), then one line per minority cluster:
`1 of 3 runs differed: <difference>` in `--tx-dim`.

## 3. Thinking traces
If the target card has `reasoning.kind: hybrid-thinking` and the run request sets
instrument `thinking`, pass `thinking_budget: 4096` to the provider call and return
`thinking` verbatim. UI: collapsible section `Its reasoning`, mono 12.5px `--tx-dim`,
machine-wash, collapsed by default.

## 4. Refine loop
`POST /api/artifacts/:slug/refine` per 03-API. UI on the artifact page: after a refine,
show the new version's PromptSheet with a `changed` marker: any annotation whose
excerpt did not exist in the previous version's body gets a small `new` chip in
`--coach`. History list gains the entry immediately.
