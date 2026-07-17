# 02 — Data formats (workspace files)

Every shape below has a zod schema in `app/server/src/schemas.ts` with the same name.

## 1. Artifact — `workspace/artifacts/<slug>/artifact.md`

Markdown with YAML frontmatter (parse with `gray-matter`). `<slug>` is the artifact
name: lowercase, `a-z0-9-` only, derived from the title by slugify (spaces→`-`, strip
other chars, collapse `-`).

```yaml
---
title: Call summary for CRM           # human title
kind: prompt                          # prompt | system_prompt | skill | agent_def | tool_desc
version: 1                            # integer, starts at 1
created: 2026-07-17T12:00:00.000Z
updated: 2026-07-17T12:00:00.000Z
target_model: claude-sonnet-5         # a model card id
variables: [call_transcript]          # strings matching {{name}} slots in the body
tier: T1                              # T0 | T1 | T2
status: green                         # green | stale | regressed | untested
last_verified: 2026-07-17T12:05:00.000Z   # nullable
task_profile:                         # exactly the object returned by the profiler
  output_verifiability: checkable     # checkable | subjective | mixed
  reasoning_depth: low                # low | medium | high
  format_sensitivity: high            # low | high
  context_volume: single_doc          # none | single_doc | large
  reuse: recurring                    # one_off | recurring
  execution: single_call              # single_call | agentic
original_ask: "I need consistent structured summaries of customer support calls"
interrogation:                        # the Q&A that shaped this artifact
  - q: "What happens to a summary after it's written?"
    a: "It's inserted into our CRM by a script"
checks:                               # see 06-CHECKS §1 for the check object shape
  - { id: c1, type: json_schema, config: { schema: { ... } }, source: "interrogation[0]" }
annotations:                          # see §1.1
  - { excerpt: "<role>", technique: role-scoped-to-reader, note: "...", model_reason: "..." }
changelog:
  - { version: 1, at: 2026-07-17T12:00:00.000Z, summary: "Initial compile from ask" }
---
<role>
You are a customer-operations analyst ...
</role>
...prompt body with {{call_transcript}}...
```

### 1.1 Annotation object
`{ excerpt: string, technique: string, note: string, model_reason?: string }`
- `excerpt` is an **exact substring** of the body (first occurrence anchors the UI
  highlight). If the excerpt is not found verbatim in the body, drop the annotation
  at save time (do not error).
- `technique` is a slug that must exist as `workspace/knowledge/techniques/<slug>.md`;
  if it doesn't, keep the annotation but render without a card link.

### 1.2 Version history — `workspace/artifacts/<slug>/history/v<N>.md`
On every save that changes the body or checks, first copy the current `artifact.md`
to `history/v<current-version>.md`, then bump `version` and write the new file.

### 1.3 Status computation (run whenever an artifact is read or listed)
- `untested`: no receipt/suite run recorded for the current version.
- `green`: last run of current version passed all checks AND `last_verified` ≤ 30 days ago.
- `stale`: `green` conditions except `last_verified` > 30 days ago.
- `regressed`: last run of current version failed ≥ 1 check.

## 2. Suite — `workspace/artifacts/<slug>/suite.yaml` (P6+)

```yaml
cases:
  - id: case-1
    vars: { call_transcript: "@fixtures/call1.txt" }   # "@" prefix = file relative to artifact dir
    checks: [c1, c2]            # ids from artifact frontmatter; empty = all checks
budget_usd_per_run: 0.50
pairwise: { samples: 3 }        # optional; see 09-LAB §4
```

## 3. Model card — `workspace/models/<id>.yaml`

Seeded files exist; the app must treat cards as data. Shape:

```yaml
id: claude-sonnet-5             # filename must equal id + ".yaml"
label: Claude Sonnet 5
provider: anthropic             # anthropic | openrouter | openai | local
api_model_id: claude-sonnet-5   # what goes in the API request
# base_url: http://localhost:11434/v1   # REQUIRED when provider is local; else absent
category: general               # general | coding | reasoning | vision | utility
card_depth: full                # full | thin — thin = catalog facts + provider description
                                #   only; UI badges it and the compiler is told
# vram_class: 8gb               # local cards only: primary tier: 4gb | 8gb | 16gb | 24gb+
# rankings:                     # local cards only: a model may rank in several categories
#   - { category: general, vram_class: 8gb, rank: 1 }
#   - { category: coding,  vram_class: 8gb, rank: 1 }
context_window: 1000000
max_output_tokens: 64000
pricing: { input_per_mtok_usd: 2.0, output_per_mtok_usd: 10.0 }
params: { effort: true, verbosity: true, temperature: false, logprobs: false }
sampling:
  default_temperature: null      # null when the model doesn't expose temperature
  notes: "..."
reasoning:
  kind: adaptive                 # none | adaptive | always-on | manual
  cot_prompting: harmful         # helpful | neutral | harmful
  notes: "..."
formatting_idiom: "..."          # free text, injected into compiler prompt
structured_output: "..."         # free text
strengths: ["..."]
failure_modes: ["..."]
prompting_notes: "..."           # free text, injected into compiler prompt
last_reviewed: 2026-07-17
sources: ["https://..."]
```

`workspace/models/<id>.measured.json` (P8): written only by the probe battery —
`{ battery_version: string, measured_at: iso, metrics: { [probe_dimension]: number } }`.

### 3.1 Catalog — `workspace/models/catalog/`
The two canonical, always-current sources of what exists (curated cards cover only the
models worth prompting deliberately):
- `openrouter.json` — trimmed snapshot of `GET https://openrouter.ai/api/v1/models`
  (no key required): `{ source, fetched, count, models: [{ id, name, created,
  context_length, prompt_usd_per_tok, completion_usd_per_tok, supported_parameters,
  modality }] }`. Refreshed automatically (10-CURRENCY §1).
- `bedrock.json` — `{ source, fetched, note, providers: { [provider]: [model names] } }`,
  maintained against the AWS Bedrock "models at a glance" page (manual review; HTML,
  no stable API).
The UI model picker offers curated cards first, then a searchable "from catalog"
section for any OpenRouter id without a card (runs with catalog pricing/params and a
generic empty-notes card).

## 4. Run record — append one JSON line per LLM call to `workspace/runs/YYYY-MM.jsonl`

```json
{
  "id": "uuid",
  "at": "iso",
  "purpose": "profile|compile|receipt_input|receipt_exec|check|interpretation|consistency|refine|coach|grade|pairwise|probe|other",
  "artifact": "call-summary-crm@1",
  "model": "claude-sonnet-5",
  "params": { "temperature": 1.0, "max_tokens": 4096 },
  "input_hash": "sha256hex",
  "prompt_chars": 1234,
  "output_chars": 567,
  "input_tokens": 1000,
  "output_tokens": 250,
  "usd_micro": 10500,
  "latency_ms": 2100,
  "cached": false,
  "output_ref": "runs/cache/<cache-key>.json"
}
```
`artifact` is nullable. Full request/response bodies live only in the cache file.

## 5. Cache file — `workspace/runs/cache/<key>.json`

Key = `hash(purpose + "|" + model + "|" + JSON.stringify(params) + "|" + hash(promptText))`.
Contents: `{ "key", "at", "request": <exact provider request body>, "response": <exact provider response body>, "output_text": string, "usage": { input_tokens, output_tokens }, "usd_micro": number }`.
Cache hit rule: same key → return stored `output_text`/usage, write a run record with
`"cached": true` and `usd_micro: 0`. Purposes `consistency` and `probe` **bypass** the
cache (sampling matters); everything else uses it.

## 6. Ledger — `workspace/runs/ledger.json`

`{ "month": "2026-07", "spent_usd_micro": 123456 }` — reset when month changes.
Updated atomically (read, modify, write) inside `llmCall()`.

## 7. Config — `workspace/config.json` (seeded)

```json
{
  "monthly_cap_usd": 25,
  "default_target_model": "claude-sonnet-5",
  "utility_model": "claude-sonnet-5",
  "cheap_model": "claude-haiku-4-5",
  "receipt_inputs": 3,
  "consistency_samples": 3,
  "stale_after_days": 30
}
```
`utility_model` runs pipeline internals (profiler, compiler, judges). `cheap_model`
runs high-volume/low-stakes calls (receipt input generation, consistency same/different
judging, coach confirmation).

## 8. Queue proposal — `workspace/queue/<id>.json` (P5+)

```json
{
  "id": "uuid", "at": "iso",
  "kind": "coach_fix|regression_fix|card_update|case_draft",
  "title": "string", "body_md": "string",
  "evidence": [{ "label": "string", "ref": "runs/observed/... or runs/2026-07.jsonl#id" }],
  "action": { "type": "write_file|update_artifact|none", "path": "string", "content": "string" },
  "status": "open|approved|rejected"
}
```
Approving executes `action` exactly and sets status; rejecting only sets status.

## 9. Observed session — `workspace/runs/observed/<source>/<session-id>.json` (P4+)

```json
{
  "id": "session id", "source": "claude-code|gateway",
  "started": "iso", "ended": "iso",
  "model_hint": "string|null",
  "turns": [ { "role": "user|assistant", "text": "string", "at": "iso|null" } ],
  "meta": { "cwd": "string|null", "total_turns": 0 }
}
```

## 10. Verdicts — append to `workspace/verdicts/bakeoffs.jsonl` (P6+)

`{ "id", "at", "artifact", "task_profile", "candidates": [{ "label", "technique_slugs": [], "score" }], "winner_label", "cases": n }`
