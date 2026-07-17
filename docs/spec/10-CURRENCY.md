# 10 — Staying current: watcher, field scan, battery, sentinel (P8)

## 1. New-model watcher
`POST /api/watch/scan` (and daily `setInterval`):
- Anthropic: GET `https://api.anthropic.com/v1/models` (same auth headers) → `data[].id`.
- OpenRouter (if key): GET `https://openrouter.ai/api/v1/models` → `data[].id`.
Compare against existing card ids + `workspace/models/.seen.json` (array of ids already
offered). For each new id: file a queue proposal `kind: card_update`, title
`New model available: <id>`, body listing the id and provider, `action.type: "none"`.
Card drafting stays user-initiated: `POST /api/models/draft { model_id, provider, sources_text }`
runs CARD_DRAFT (04-PROMPTS §10) with `claude-sonnet-5.yaml` as the example card and
returns `card_yaml` for review; `POST /api/models { card_yaml }` validates and writes
the file. (`sources_text` is pasted by the user — release notes, docs. No web scraping
in this build.)

## 2. Field scan
Out of scope for automation in this build (no unattended web fetching). The UI Models
page shows per-card staleness (05-UI §7); a `Review guide` link renders
`workspace/knowledge/practices.md` so the user refreshes cards deliberately.

## 3. Probe battery
`workspace/knowledge/probes/battery-v1.yaml` (seeded, 12 probes; extend by the same
format — each probe MUST be deterministically scorable):

```yaml
version: v1
probes:
  - id: fmt-json-1
    dimension: format_adherence
    prompt: 'Return exactly this JSON with the number field set to 7: {"status": "ok", "number": 0}. Return only JSON.'
    score: { type: json_schema, config: { schema: { type: object, required: [status, number], properties: { status: { enum: [ok] }, number: { type: number } } } } }
  # dimensions used: format_adherence, instruction_following, position_sensitivity, hedging
```

`POST /api/models/:id/battery`: run every probe on the model (purpose `probe`, cache
bypass, temperature 0), score with the probe's check, aggregate per dimension =
mean pass (for `hedging`, the probes are phrased so pass = did NOT hedge; report
`hedging_rate = 1 - mean`). Write `.measured.json` (02-DATA §3). Cost estimate shown in
UI before running. Cross-version rule: gauges display only metrics whose
`battery_version` matches the current battery file version.

## 4. Regression sentinel
Nightly job (`setInterval`, 24h, first run 60s after start):
1. Collect artifacts with a suite whose `status` is `stale`, or whose target model card
   file changed since `last_verified` (compare mtime).
2. Budget guard: stop when the night's sentinel spend reaches
   `min(2, monthly remaining × 0.1)` USD.
3. Run each suite. All-pass → status green (silent). Any fail → status regressed +
   fix-candidate flow:
   run REFINER (04-PROMPTS §7) with feedback =
   `The following checks failed on the test suite: <check labels + details>. Fix the prompt so they pass without breaking anything else.`
   → run the suite against the refined body → if it strictly beats the incumbent
   (more passes), file queue proposal `kind: regression_fix` with
   `action: { type: "update_artifact", path: <slug>, content: <new body> }` and the
   before/after numbers in `body_md`; else file a `regression_fix` proposal with
   `action.type: "none"` and body `regressed, no fix found`.
Approving an `update_artifact` proposal = snapshot history, bump version, changelog
`Applied sentinel fix`, re-run suite, set status accordingly.
