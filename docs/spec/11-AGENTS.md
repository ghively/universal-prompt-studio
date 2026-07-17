# 11 — Agent wind tunnel (P9, heaviest; requires `claude` CLI on PATH)

Applies to artifacts with `kind: skill | agent_def`. A skill artifact's directory
contains `SKILL.md` (the artifact body is SKILL.md's content) plus optional resources.

## 1. Scenarios — `workspace/artifacts/<slug>/scenarios.yaml`

```yaml
scenarios:
  - id: s1
    name: should-trigger-on-pdf-task
    setup_files: { "report.pdf": "@fixtures/report.pdf" }   # copied into a temp dir
    task: "Summarize this PDF's financials into summary.md"
    expect:
      activation: required          # required | forbidden | any
      compliance: { criterion: "The transcript shows the agent followed the skill's numbered steps in order", min: 0.7 }   # optional
      outcome:                      # deterministic checks against the temp dir after the run
        - { type: file_exists, path: "summary.md" }
        - { type: file_contains, path: "summary.md", value: "revenue" }
    max_turns: 20
    budget_usd: 1.00
```

## 2. Runner
`POST /api/artifacts/:slug/scenarios/run` body `{ "ids"?: [..] }`. Per scenario:
1. Create temp dir; copy setup files; for `kind: skill`, install the artifact at
   `<tmp>/.claude/skills/<slug>/SKILL.md`; for `agent_def`, write body to
   `<tmp>/CLAUDE.md`.
2. Spawn: `claude -p "<task>" --output-format stream-json --max-turns <max_turns>`
   with `cwd = tmp dir`, env inherited. Kill after 10 min or when stdout closes.
   Collect the stream-json lines as the transcript; store to
   `workspace/runs/agent/<slug>/<scenario>-<ts>.jsonl`.
3. Score:
   - **activation**: transcript contains a line whose JSON stringified form includes
     the skill slug (tool-use of the Skill tool or skill announcement). `required` →
     pass if found; `forbidden` → pass if absent; `any` → pass.
   - **compliance** (optional): RUBRIC_JUDGE (04-PROMPTS §5.2) on `utility_model` with
     OUTPUT = the transcript's assistant text turns concatenated (truncate to the final
     30,000 chars) and the scenario's criterion.
   - **outcome**: `file_exists` / `file_contains` (substring, case-insensitive) checks
     against the temp dir.
4. Scenario passes iff all three axes pass. Delete the temp dir.
Response: `{ "scenarios": [ { id, name, activation: bool, compliance: {score,pass}|null, outcome: [ {path,pass} ], pass } ], "usd" }`.
Budget: skip (as failed, detail `budget`) any scenario whose `budget_usd` is exceeded
mid-run — check ledger delta after the spawn completes.

## 3. UI (`/library/:slug` for skill/agent kinds)
Scenario list with per-axis verdict chips (activation / compliance / outcome) colored
pass/fail; row expands to the transcript (user turns `--human`, assistant `--tx-dim`,
tool lines `--machine`, 12.5px mono). `Run scenarios ($est)` gold pill.
Status computation for these kinds uses scenario results in place of checks.
