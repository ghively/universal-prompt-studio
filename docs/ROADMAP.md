# Roadmap — from empty repo to the product in PRODUCT.md

*Priority order set by the owner: Ask mode is the front door; Coach mode (observed
usage) is the most-wanted differentiator; Lab mode is where artifacts graduate.
Every milestone ships something usable on its own. Effort is estimated in solo
weekends; building with Claude Code compresses each substantially — every milestone
here is a well-scoped unit for agent-built development.*

## The shape of the whole thing

| # | Milestone | You can now… | Effort | Depends on |
|---|---|---|---|---|
| M0 | Repo reset | — | ½ day | — |
| M1 | Ask mode core | describe a task → get an explained, model-aware prompt | 1–2 wkd | M0 |
| M2 | Receipt + smoke checks | see *proof* it's better; library with health badges | 1–2 wkd | M1 |
| M3 | Instruments | see the model's reaction; refine iteratively | 1 wkd | M2 |
| M4 | Observation layer | the tool sees your real Claude Code / agent usage | 1–2 wkd | M1 |
| M5 | The coach | weekly digest + one-click fixes from real usage | 2–3 wkd | M4, M2 |
| M6 | Lab mode | curated suites, arena bake-offs, ablation, promotion | 2–3 wkd | M2 |
| M7 | Staying current | new-model watcher, field scan, probe battery, sentinel | 2–3 wkd | M6 |
| M8 | Agent wind tunnel | scenario-eval skills, CLAUDE.md files, agent defs | 2–4 wkd | M6 |
| M9 | Surfaces | MCP server, CLI, CI mode | 1–2 wkd | M2 (MCP can land any time after) |

Total: roughly 12–20 solo weekends end-to-end; v1.0 = M1–M9 complete. The exchange
(NORTHSTAR §7) is explicitly post-1.0.

---

## M0 — Repo reset (half a day)

- Archive `universal-prompt-studio-v11.html` under `legacy/` (directional reference,
  per owner: zero attachment).
- Scaffold: `server/` (TypeScript, Hono), `web/` (React + Vite), `workspace/` (the
  data model from PRODUCT.md §2), `.env.example` for keys.
- **Exit criterion:** `npm run dev` serves a hello-world UI from the local server.

## M1 — Ask mode core (1–2 weekends)

The seed of the product, and the owner's stated primary loop.

- Provider adapter (Anthropic + OpenAI + OpenRouter; AI SDK first, raw SDKs where it
  hides fields).
- **Knowledge seed, hand-written:** ~15 technique cards + 5 model cards. These are
  content, not code — and they're load-bearing from day one because generation cites
  them.
- The flow: task description → profiler picks 2–4 interrogation questions → answers
  captured as acceptance criteria → **one** generated prompt → **annotated
  explanation** (each section linked to its technique card + the model-card reason).
- Save artifact to `workspace/artifacts/`; every call logged to `runs/`.
- **Exit criterion:** type "I need consistent structured summaries of customer
  calls," answer 3 questions, receive an explained prompt targeting a chosen model,
  save it, and find it on disk as a markdown file.
- Running cost: pennies per ask.

## M2 — The receipt + smoke checks (1–2 weekends)

"Better" becomes demonstrated, never asserted — the anti-vibes core.

- Auto-generate 2–3 representative inputs from the task profile; run your original
  phrasing vs. the generated prompt side by side; highlight the differences.
- Acceptance criteria (from M1 interrogation) compile into smoke checks
  (deterministic first: schema/contains/format; rubric only where unavoidable).
- Verification tiers T0/T1 live; library view with health badges (green/stale);
  re-run button.
- Run cache keyed on `(artifact-hash, model, params, input-hash)`; always-visible
  cost meter; global monthly budget cap enforced server-side.
- **Exit criterion:** every saved artifact shows a receipt and passes/fails its smoke
  checks; re-running an unchanged artifact costs $0 (cache hit).

## M3 — Instruments (1 weekend)

The look-behind-the-curtain layer, attached to any run.

- Interpretation report (structured side-call: restate task, ambiguities,
  assumptions, conflicts) — labeled as self-report.
- Consistency probe (N samples, agreement score, divergence view).
- Thinking-trace rendering for models that expose it.
- Refine loop: "this output is wrong because…" → regenerate with the explanation
  updated (a new version, changelog written).
- **Exit criterion:** run any saved artifact and read all three instruments; refine
  it and see version 2 with a diff.

## M4 — Observation layer (1–2 weekends)

Coach mode's senses — capture only, no judgment yet.

- **Claude Code transcript ingestion first** (zero-config: read local JSONL under
  `~/.claude/projects/`), then the **gateway proxy** (local OpenAI/Anthropic-
  compatible endpoint; point any tool's `base_url` at it; full request/response
  logged). MCP-proxy adapter last, if needed.
- Observed store under `runs/observed/` with a browsing UI + basic stats (sessions,
  models used, tokens, spend).
- Privacy defaults: local-only, redaction config, observed store excluded from any
  export path.
- **Exit criterion:** yesterday's real Claude Code session is browsable in the UI;
  a curl through the proxy shows up within seconds.

## M5 — The coach (2–3 weekends)

The differentiator. Start with few, reliable detectors — not many flaky ones.

- Detector set v1: retry/rephrase loops; chronic underspecification (which dimension
  you habitually omit); instructions agents repeatedly ignore; spend hotspots.
- Weekly digest + proposals into a review queue; each proposal links into Ask mode
  to generate the fix (e.g. a CLAUDE.md block), which arrives with its own receipt.
- Observed real inputs become *drafted* test cases attached to matching library
  artifacts (approval required).
- **Exit criterion:** after a normal week of usage, one digest with at least one
  actionable, evidence-linked proposal you actually apply.
- Risk note: detector precision matters more than recall — a coach that cries wolf
  gets ignored. Ship each detector with a "was this useful?" feedback control and
  drop the ones you dismiss.

## M6 — Lab mode (2–3 weekends)

Where artifacts graduate (T2). Built on M2's check machinery.

- Curated suites (cases × checks, per-suite budgets); grader artifacts with anchor
  labels; pairwise judging (position-swap, 3 samples), you-as-blind-judge included,
  judge-vs-you agreement tracked.
- Arena: candidates × models × cases grid; promote-to-incumbent with version
  snapshot.
- Ablation prober (drop/reorder/paraphrase variants vs. the suite).
- Graduation flow: T1 artifact + coach-drafted cases → T2 in one review session.
- **Exit criterion:** take your most-used prompt to T2, run a 3-candidate bake-off,
  and promote a winner — with the whole evidence trail browsable.

## M7 — Staying current (2–3 weekends)

The self-updating knowledge loop.

- New-model watcher (daily provider-list diff → card draft from release notes →
  review queue).
- Field-scan job (periodic re-read of provider docs/changelogs → technique/model
  card update drafts).
- Probe battery v1: 20–40 deterministic probes (instruction-following under
  distractors, format adherence, position sensitivity, hedging); `measured.json`
  beside every curated card; battery versioned.
- Regression sentinel (nightly, budget-capped, cache-warm) + fix candidates (only
  filed when they beat the incumbent on the same cases).
- **Exit criterion:** simulate a model release (add a card stub): battery runs,
  library re-verifies, digest proposal appears — end to end without manual steps.

## M8 — Agent wind tunnel (2–4 weekends)

- `kind: skill / agent_def / tool_desc` harness: scenario files, headless runs via
  the Claude Agent SDK, activation/compliance/outcome scoring, per-scenario budgets.
- Trajectory viewer with diffing between artifact versions.
- **Exit criterion:** a real skill you use passes a should-trigger and a
  must-not-trigger scenario, and a deliberate wording regression is caught.
- This is the heaviest, flakiest milestone (sandboxing, nondeterminism); it sits
  late deliberately, but only depends on M6 — pull it forward if skill evaluation
  becomes the priority.

## M9 — Surfaces (1–2 weekends)

- MCP server: `search_artifacts`, `render`, `model_brief`, `log_outcome` — your
  other agents draw from and feed the workspace. (Cheap; can be built any time
  after M2 — it's listed last only because nothing else depends on it.)
- CLI (`ps ask / run / eval / bench / bakeoff`) sharing the server's API; CI mode
  with exit codes + markdown scoreboard.
- **Exit criterion:** Claude Code, via MCP, retrieves and uses one of your library
  prompts inside a real session, and logs the outcome back.

---

## Rules of the road (apply to every milestone)

1. **Ship usable or don't merge.** Each milestone ends on its exit criterion, not on
   "mostly done."
2. **Knowledge cards are living from M1.** Any milestone may add/update cards;
   staleness dates from day one.
3. **Budgets before features.** No milestone adds a spend path without a cap.
4. **Proposals, never auto-apply.** Daemon output goes to the review queue. Always.
5. **The workspace is sacred.** Any version of the app must run against a workspace
   created by any earlier version — files, not migrations, wherever possible.
