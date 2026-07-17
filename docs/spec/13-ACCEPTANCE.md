# 13 — Acceptance checklist (build order — work top to bottom, check boxes as you go)

Rules: complete each phase's items in order; a phase is done when every box is checked
and its gate test passes; commit per item (`P<n>: <item> (spec §ref)`). Tests that call
providers need `app/.env` with a real `ANTHROPIC_API_KEY`; if it is absent, mark the
item `[skipped-no-key]` instead of checking it and continue — but never mark the phase
gate done without keys.

## P0 — Scaffold (01-STACK)
- [ ] `app/` created exactly per 01-STACK §1–5 (package.json, tsconfig, vite config, .env.example verbatim)
- [ ] `npm install` succeeds; `npm run check` passes on empty skeleton
- [ ] Server starts; `curl localhost:5680/api/health` → `{"ok":true,...}`
- [ ] Workspace bootstrap creates missing directories (delete `workspace/runs` and restart to verify)
- [ ] `npm run dev` serves the UI shell at :5173 with tokens.css applied (dark canvas, dot grid visible)
**Gate:** all above on a fresh clone.

## P1 — Ask core (03-API §1, 04-PROMPTS §2–3, 05-UI §3 states 1–5)
- [ ] zod schemas for config, model card, run record, artifact frontmatter (02-DATA) + unit tests with one valid/one invalid fixture each
- [ ] `providers.ts` with all three providers; unit test the request-body builders (no network)
- [ ] `llmCall()` with budget guard, cache, ledger, run records; unit tests: cache hit returns usd 0; guard throws over cap
- [ ] GET /api/models returns the 5 seed cards with `available` flags
- [ ] POST /api/ask/profile returns 2–4 questions for the ask `"I need consistent structured summaries of customer support calls"` (live test)
- [ ] POST /api/ask/compile returns a draft whose body contains at least one `{{variable}}`, ≥ 2 annotations with verbatim excerpts, ≥ 2 checks (live test)
- [ ] Ask page states idle→drafted work end to end in the browser; annotations cross-highlight on hover and focus
**Gate:** type the ask above in the UI, answer the questions, and read an annotated compiled prompt with technique card links opening the side sheet.

## P2 — Receipt, save, library (03-API §1, 06-CHECKS, 05-UI §3 states 6–8, §5–6)
- [ ] `checks.ts`: all deterministic types + unit tests (incl. restricted json_schema validator: 6 cases)
- [ ] LLM-judged checks quote_support and rubric (live test each once)
- [ ] POST /api/ask/receipt returns rows + totals for a 2-input run (live)
- [ ] Receipt UI: compare grid, per-input selector, tally, verbatim budget-exceeded error rendering (force by setting cap to 0.000001)
- [ ] POST /api/artifacts writes a valid artifact.md; GET list/detail; PUT snapshots history and bumps version (unit-test the snapshot logic on a tmp workspace)
- [ ] Library page with attention sort and stale fade; artifact detail with run block executing checks
**Gate:** full flow ask→interrogate→compile→receipt→save; artifact appears in library green; `runs/<month>.jsonl` has records for every call; re-running the identical receipt costs $0 (cache).

## P3 — Instruments & refine (07-INSTRUMENTS)
- [ ] Interpretation report renders with the self-report caveat line (live)
- [ ] Consistency probe: 3 samples, clustering via SAME_ANSWER_JUDGE, agreement figure (live)
- [ ] Thinking trace shown for `claude-sonnet-5` runs with instrument enabled (live)
- [ ] Refine bumps version, marks new annotations, history renders old versions read-only
- [ ] (stretch, optional) SSE streaming for run output
**Gate:** on a saved artifact, one click yields output + all three instruments + checks, and a refine produces v2 with visible changelog.

## P4 — Observation (08-COACH §1–2)
- [ ] Ingest parses a fixture Claude Code JSONL (commit a small synthetic fixture under `app/server/src/fixtures/`) into an observed session (unit)
- [ ] Ingest of real `~/.claude/projects` when present; idempotent re-ingest (state file)
- [ ] Sessions list + expandable turns UI
- [ ] Gateway proxy relays a real Anthropic call unchanged and records the exchange (live; use curl with the base URL override)
**Gate:** yesterday's real session (or the fixture) is browsable at /coach.

## P5 — Coach (08-COACH §3)
- [ ] Detectors D1–D4 as pure functions with unit tests on synthetic sessions (each detector: one firing case, one non-firing)
- [ ] FINDING_CONFIRM gate + digest file writing (live once with synthetic evidence)
- [ ] Digest UI as sticky findings; FIX_DRAFT flow files a queue proposal; queue page approve/reject
- [ ] Weekly scheduler + per-finding usefulness feedback endpoint
**Gate:** POST /api/coach/digest on fixture sessions produces a digest with ≥ 1 confirmed finding and a drafted fix in the queue.

## P6 — Lab (09-LAB)
- [ ] Suite read/write/run with @fixture resolution and per-run budget refusal (unit + live)
- [ ] Case drafting from observed sessions
- [ ] Rubric anchor calibration flow with MAE warning
- [ ] Bake-off: checks + position-swapped pairwise, verdict record, promote (live with 2 candidates × 2 cases)
- [ ] Blind human judging + agreement rate
- [ ] Ablation endpoint + load-bearing table UI (live on an artifact with a 2-case suite)
**Gate:** run a bake-off where a deliberately weakened candidate loses to the incumbent, and an ablation that marks at least one section load-bearing.

## P7 — Surfaces (12-SURFACES)
- [ ] MCP stdio server: tools/list + all four tools callable (test with a scripted JSON-RPC stdin session)
- [ ] `log_outcome` regression rule (3 consecutive fails → regressed) unit-tested
- [ ] CLI: ask/run/eval/eval --all/bench with correct exit codes (unit-test arg parsing; live-test `run`)
**Gate:** from a Claude Code session, `render_prompt` returns a library prompt via MCP; `cli eval --all` exits 0 on a green library.

## P8 — Currency (10-CURRENCY)
- [ ] Watcher diffs live model lists into queue proposals (live; also unit-test the diff)
- [ ] CARD_DRAFT flow: paste sources → reviewed YAML → new card file
- [ ] Battery v1 runs and writes measured.json; gauges appear on /models with battery version + date (live)
- [ ] Sentinel job: stale-suite re-verification + fix-candidate proposal (simulate by editing a check to force failure); nightly budget stop
**Gate:** force a regression, wait for (or trigger) the sentinel, approve the fix proposal from the queue, artifact returns to green.

## P9 — Wind tunnel (11-AGENTS; requires `claude` CLI)
- [ ] Scenario schema + runner with temp-dir setup/teardown (unit-test scoring on a canned transcript)
- [ ] Activation required/forbidden both verified live with a trivial skill
- [ ] Compliance + outcome axes live; per-axis UI chips; transcript viewer
**Gate:** a real skill passes a should-trigger and a must-not-trigger scenario; breaking its description makes activation fail.

## Done = v1.0
All gates green. Update README.md status line to "v1.0 — built" (the only README edit allowed).
