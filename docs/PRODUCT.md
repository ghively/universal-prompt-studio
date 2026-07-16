# The Product — concrete end-state specification

*NORTHSTAR.md is the pitch. This is the machine: data model, surfaces, jobs,
integrations, costs, and the honest hard parts. No adjectives that don't compile.*

---

## 1. What it physically is

Three processes and a directory:

| Piece | What it is |
|---|---|
| **Workspace** | A git repo of plain files. The only source of truth. Delete the app, keep the workspace, lose nothing. |
| **Server** | One local TypeScript process (Hono). Owns API keys, provider adapters, the run cache, and the job queue. Serves the UI and the API. |
| **UI** | React app served by the server. Six screens (§3). |
| **Daemon** | The same server process running scheduled jobs (§4) under a hard monthly budget cap. |

Plus two thin clients over the server's API: a **CLI** (`ps …`) and an **MCP server**
exposing the workspace to other agents (§5).

## 2. Data model (all files, all diffable)

```
workspace/
├── artifacts/            # the things under test
│   └── <name>/
│       ├── artifact.md   # frontmatter + body (see below)
│       ├── suite.yaml    # its test cases + checks
│       └── history/      # frozen snapshots at each promoted version
├── models/
│   └── <id>/
│       ├── card.yaml     # curated: idiom, quirks, notes, sources, last_reviewed
│       └── measured.json # written ONLY by probe runs: fingerprint metrics + dates
├── knowledge/
│   ├── practices.md
│   ├── techniques/*.md   # technique cards (lever, mechanism, helps/hurts, example)
│   └── probes/*.yaml     # versioned probe batteries (§4.1)
├── runs/
│   └── YYYY-MM/*.jsonl   # append-only: every execution, cost, params, hashes
├── verdicts/*.jsonl      # bake-off results linked to task profiles (the evidence base)
├── journal/*.md          # half-auto-written entries; calibration predictions + reveals
└── queue/                # proposals awaiting human review (§4.4)
    └── <id>/{proposal.md, diff.patch, evidence.json}
```

**Artifact** — one type covers prompts, system prompts, skills, agent defs, tool
descriptions; `kind:` switches the harness:

```yaml
# artifacts/contract-extractor/artifact.md (frontmatter)
kind: prompt            # prompt | system_prompt | skill | agent_def | tool_desc
version: 14
variables: [contract_text, jurisdiction]
targets: [claude-sonnet-5, gpt-5.2]
task_profile: {verifiability: checkable, reasoning: low, format_sensitivity: high, ...}
status: green           # green | stale | regressed  (computed, cached here)
changelog: [...]
```

**Suite** — cases × checks:

```yaml
cases:
  - vars: {contract_text: "@fixtures/acme.txt", jurisdiction: "DE"}
    checks:
      - {type: json_schema, schema: "@schemas/extraction.json"}   # deterministic
      - {type: contains, value: "termination_date"}               # deterministic
      - {type: rubric, grader: g-extraction-quality, min: 0.8}    # LLM judge
pairwise: {judge: g-pairwise-default, positions: swap, samples: 3}
budget: {max_usd_per_run: 0.50}
```

**Skill scenario** (when `kind: skill`) — replaces cases with trajectories:

```yaml
scenarios:
  - name: should-trigger-on-pdf-task
    setup: {files: ["@fixtures/report.pdf"]}
    task: "Summarize this PDF's financials"
    expect:
      activation: required        # skill must load
      compliance: {rubric: g-followed-skill-steps, min: 0.7}
      outcome: [{type: file_exists, path: "summary.md"}]
  - name: must-not-trigger-on-csv
    task: "Sum column B of data.csv"
    expect: {activation: forbidden}
runner: {agent: claude-agent-sdk, max_turns: 20, budget_usd: 1.00}
```

**Run record** — one JSONL line: `{ts, artifact@version-hash, model, params, input-hash,
output, tokens, usd, latency, checks: [...], trace_ref}`. The cache key is
`(artifact-hash, model, params, input-hash)` — identical work is never paid for twice.

## 3. The six screens

1. **Workbench** (daily driver): editor left; run controls right (model picker fed by
   cards, params prefilled); bottom instrument strip — output, thinking trace,
   interpretation report, consistency score, logprob heatmap (when available). Every
   lint/critique annotation links to its technique card.
2. **Library**: artifact table — kind, status badge, pass rate, last verified, models
   verified against, cost/run. Click-through to version timeline with score-per-version.
3. **Arena**: a bake-off is a grid — candidates × models × cases — filling live.
   Columns: deterministic checks, rubric scores, pairwise wins, cost, latency. One
   button: *promote candidate → incumbent* (writes a version snapshot + changelog).
4. **Models**: per model, curated card beside **measured fingerprint** (radar of probe
   scores + date measured + battery version). Staleness badges. "Re-measure" button
   with cost preview.
5. **Review Queue**: proposals from daemon jobs (§4.4) as PR-style diffs with attached
   evidence (the runs that justify it). Approve / reject / edit. Nothing auto-applies.
6. **Progress**: calibration score over time (predictions vs. reveals), curriculum
   position, journal search, monthly spend by category.

## 4. The daemon (all jobs → proposals, never auto-apply)

### 4.1 Probe batteries → `measured.json`
A battery is a versioned YAML suite of ~40–80 small tasks with deterministic scoring,
measuring: instruction-following under distractors, format adherence, context-position
sensitivity (needle tasks at varied depths), hedging rate, sycophancy flip-rate,
verbosity drift, sampling stability. Output: numbers + battery version + date. Cost:
roughly $1–5 per model per run. Batteries are versioned so fingerprints stay comparable.

### 4.2 New-model watcher
Diffs provider model lists daily. On a new model: draft `card.yaml` from release
notes/docs (LLM-drafted, human-reviewed), run the battery, run library suites for
artifacts whose `targets` policy says "try new models", file one digest proposal:
holds / improved / regressed, with diffs and costs.

### 4.3 Regression sentinel
Nightly, within budget: re-verify artifacts whose (a) model card changed, (b) suite
changed, (c) `status: stale` (untested > N days). Cache makes unchanged work free.

### 4.4 Fix candidates
For each regression: generate 2–3 repair candidates (critique pass, model-aware),
run them against the suite, file the best as a proposal **only if it beats the
incumbent** on the same cases. Otherwise file a "regressed, no fix found" report.
This is the honest mechanics behind "self-healing": automated PR authorship,
human merge.

## 5. Integration surfaces

- **MCP server** (the workspace becomes agent-readable):
  `search_artifacts(query, kind)`, `render(artifact, vars, model)` → compiled prompt,
  `model_brief(id)` → card + fingerprint summary, `log_outcome(artifact, result)` →
  feeds the evidence base from real-world use. Read tools are safe; `log_outcome` is
  append-only.
- **CLI**: `ps run <artifact> --model X --vars f.json`, `ps eval <artifact>`,
  `ps bench <model>`, `ps bakeoff <artifact> --candidates 3`. Same API as the UI.
- **CI mode**: `ps eval --all --json` with exit codes; a GitHub Action posts the
  scoreboard as a PR comment when prompts change in a repo.

## 6. Costs (order-of-magnitude honesty)

| Operation | Typical cost |
|---|---|
| Single run + instruments (trace + interpretation + 3-sample consistency) | $0.01–0.10 |
| Suite run (10 cases, mixed checks), cold cache | $0.10–1 |
| Bake-off (3 candidates × 10 cases × pairwise ×3) | $1–5 |
| Ablation probe (8 variants × suite) | $1–8 |
| Probe battery per model | $1–5 |
| Skill scenario suite (10 trajectories) | $2–20 (agentic runs dominate) |
| Nightly sentinel, warm cache, 30-artifact library | often < $1/night |

A global monthly cap and per-suite budgets are enforced by the server, not by hope.

## 7. The hard parts (where the engineering actually is)

1. **LLM graders are noisy.** Mitigations: prefer deterministic checks wherever
   possible; pairwise with position-swap and 3 samples; grader prompts are themselves
   artifacts with their own suites (graders grading graders bottoms out in a small
   set of human-labeled anchor cases you create once per grader).
2. **Eval sets are the real bottleneck.** Verdicts are only as good as the cases. The
   tool helps (case generation from your prompt + variable space, coverage hints:
   "no case exercises the empty-input path"), but curating ~10 good cases per
   artifact is *your* recurring work. This is the product's honest cost of ownership.
3. **Probe battery validity.** A battery must measure the model, not itself. Design
   rule: deterministic scoring only, multiple phrasings per probe dimension, and
   battery versioning so cross-model comparisons only happen within a version.
4. **Agent trajectory grading** is expensive and flaky (sandboxing, nondeterminism).
   Contained by: scenario budgets, outcome checks on artifacts (files, exit states)
   over transcript vibes, and compliance rubrics as secondary signal only.
5. **The exchange** (NORTHSTAR §7) requires other people; it ships last and the
   product must be complete without it. Everything else here is single-player.

## 8. Division of labor — the automation is lab equipment; you are the scientist

Nothing in §4 creates anything. Every automated job is measurement or drudgery;
every decision that shapes an artifact is human. Explicitly:

| You (the craft) | The tool (the instruments) |
|---|---|
| Write and revise the prompts. The workbench is an editor first; most sessions are typing, running, reading the reaction panel, revising. | Make the model's reaction visible so revision is informed, not blind. |
| Define what "good" means: author/approve every test case and check. The tool drafts cases; you edit or reject each one. | Execute your definition of good, thousands of times, identically. |
| Hand-label the anchor cases that calibrate each grader. Your taste is the ground truth of the whole system. | Track each grader's agreement with your labels and flag drift. |
| Judge blind pairwise rounds yourself when it matters. | Track your agreement with LLM judges — so you learn *which graders to trust, and when*. |
| Predict outcomes before bake-offs run (predict-then-reveal). | Score your calibration over time. |
| Review and merge every proposal in the queue. | Author proposals with evidence attached; never apply anything. |
| Decide promotion in the Arena. | Fill in the scoreboard. |

**Automation is opt-in per artifact.** The daemon only touches artifacts explicitly
marked for maintenance. Something you're actively crafting has zero automation on it.
The nightly machinery exists for the boring re-verification you'd never do by hand on
*finished* work — not for creation.

**On "hoping the tests are accurate":** tests here are not an oracle you trust —
they are *your hypotheses, made executable*. When a result surprises you, that's the
mechanism working: either the prompt is wrong or the test is wrong, and you
adjudicate; both are versioned, both improve. The tool never says "this prompt is
good." It says: *on these 12 cases you wrote, under these checks you approved,
candidate B beat A 9–3, at $1.40 — transcripts here.* The claim is small, auditable,
and one click from the raw evidence. Distrust of graders is designed in
(deterministic checks preferred; graders have their own suites; §7.1), and the
useful skill you build — noticing when a metric is lying — is prompt engineering's
actual expert move.

Writing the eval **is** the skill. Deciding what a good output looks like, hunting
edge cases, phrasing a rubric a judge can apply consistently — that's where the
learning compounds, and it's why case curation is deliberately left as your work
(§7.2) rather than automated away.

## 9. Size estimate

Single-user local product: roughly **25–40k lines of TypeScript** (server 40%, UI 40%,
CLI/MCP/probes 20%), no infrastructure beyond the user's machine and their API keys.
A large personal project or a small commercial one — not a fantasy that needs a team
of thirty. Every stage in the roadmap (VISION.md §6) ships a usable tool; this
document is the shape it converges to.
