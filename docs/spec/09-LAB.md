# 09 — Lab: suites, graders, arena, ablation (P6)

## 1. Suites
`GET/PUT /api/artifacts/:slug/suite` — read/write `suite.yaml` (02-DATA §2). `@`-prefixed
var values resolve to files in the artifact directory (`fixtures/` subdir by
convention); missing file → `bad_request` naming the path.

`POST /api/artifacts/:slug/suite/run` body `{ "model"?: id }` (default = artifact
target). For each case: render body with vars → run (purpose `receipt_exec`, cached) →
run the case's checks. Respect `budget_usd_per_run`: estimate before starting; if the
estimate exceeds it → `bad_request` with the estimate. Response:
`{ "cases": [ { id, pass, checks: [...] } ], "passed": n, "of": n, "usd": number }`.
Updates artifact `status`/`last_verified` (all-pass = green).

## 2. Case drafting (coach assist)
`POST /api/artifacts/:slug/suite/draft-cases` — takes the artifact's `original_ask`,
finds observed sessions (08-COACH) whose first_ask has Jaccard ≥ 0.3 similarity with
it, extracts up to 5 user inputs as candidate case vars via INPUTGEN with the session
excerpts appended to the user message under `REAL EXAMPLES (imitate their character):`.
Returns drafted cases; user approves/edits in UI before they are written to the suite.

## 3. Grader calibration
Graders stay simple in this build: the rubric criterion lives in the check config
(no separate grader artifacts). Anchors: `workspace/evals/anchors/<artifact-slug>/<check-id>.jsonl`,
each line `{ "output": str, "human_score": 0|0.5|1 }`, created in the UI (artifact page,
`Calibrate` link on rubric checks: shows 3 recent outputs, user scores each).
`POST /api/checks/calibrate { slug, check_id }` → runs RUBRIC_JUDGE on every anchor,
reports mean absolute error; if MAE > 0.25, UI shows `this grader disagrees with you —
tighten the criterion wording` in `--fail`.

## 4. Pairwise & bake-off (arena)
`POST /api/arena/bakeoff` body:
`{ "slug": str, "candidates": [ { "label": str, "body": str } ], "cases"?: [ids], "samples"?: 3 }`
(2–3 candidates; candidate A is always the current artifact body, labeled `incumbent`).
For every case × candidate: run output (cached). Deterministic checks score first:
candidate ranking metric = mean pass rate. Then pairwise: for each case and each
candidate pair, run PAIRWISE_JUDGE (04-PROMPTS §5.4) `samples` times, **alternating
output order each sample** (position swap); `TASK` = artifact original_ask, `CRITERIA`
= concatenated check labels. A pair's winner = majority (ties count half to each).
Response: `{ "table": [ { label, check_pass_rate, pairwise_wins, cost_usd } ], "winner": label }`
— winner = highest check_pass_rate; pairwise_wins breaks ties. Append verdict record
(02-DATA §10) including each candidate's annotation technique slugs.
`POST /api/arena/promote { slug, body }` — snapshot history, bump version, changelog
`Promoted bake-off winner`.
UI `/lab/:slug`: candidates as columns filling as results land; winner column gets a
pass-wash; gold `Promote` on the winner if it isn't the incumbent.

## 5. Blind human judging
In the bake-off UI, before scores reveal: optional `Judge blind` flow — shows output
pairs unlabeled, user picks better/tie; stored to
`workspace/evals/human-judgments.jsonl` `{ at, slug, case, pair: [labels], human: label|“tie”, llm: label|"tie" }`;
after reveal, show agreement rate `you agreed with the judge N of M times`.

## 6. Ablation
`POST /api/artifacts/:slug/ablate` — requires a suite with ≥ 2 cases (else
`bad_request`: `ablation needs a suite with at least 2 cases`). Run ABLATION_VARIANTS
(04-PROMPTS §9) with the artifact's annotation excerpts → for each variant, run the
suite (cached) → response:
`{ "baseline": { passed, of }, "variants": [ { label, kind, target_excerpt, passed, of, delta } ] }`.
UI: table sorted by delta asc; rows with delta < 0 (removal hurt → load-bearing) tinted
pass-wash and labeled `load-bearing`; delta = 0 removal rows labeled `dead weight?` in
`--coach`; reorder row labeled `order sensitivity`.
