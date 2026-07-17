# 03 — Server API, provider adapter, budget, cache

Base path `/api`. All bodies JSON. Error shape per README convention 5.

## 1. Core endpoints (P1–P3)

### GET /api/health
→ `{ "ok": true, "version": "0.1.0", "workspace": "<abs path>" }`

### GET /api/config → contents of config.json merged with `{ month_spent_usd: number }`
### PUT /api/config — body = partial config; validate keys; write; return full config.

### GET /api/models
→ `{ "models": [ <card>, ... ] }` — every `models/*.yaml`, each with added fields
`measured` (contents of `.measured.json` or null) and `available` (true if the
provider's API key is configured).

### GET /api/models/:id → single card (404 `not_found` if missing).

### POST /api/ask/profile
Body: `{ "ask": string }` (non-empty, ≤ 2000 chars).
Runs PROFILER (04-PROMPTS §2) on `utility_model`.
→ `{ "task_profile": {...}, "questions": [ { "id": "q1", "text": str, "why": str, "expected_check": "none|json_schema|contains|quote_support|rubric|max_length" } ] }`

### POST /api/ask/compile
Body: `{ "ask": string, "task_profile": {...}, "answers": [ { "q": str, "a": str } ], "target_model": string }`
Server assembles COMPILER prompt (04-PROMPTS §3) with: the model card YAML, the
technique index (§3.1 below), practices.md, the ask, profile, and answers. Runs on
`utility_model`.
→ `{ "draft": { "title": str, "body": str, "variables": [str], "annotations": [...], "checks": [...] }, "usd": number }`
Post-processing (mechanical): drop annotations whose `excerpt` isn't a verbatim
substring of `body`; drop checks with unknown `type`; ensure every `{{var}}` in body
appears in `variables` (add if missing).

**3.1 Technique index**: concatenation of every `knowledge/techniques/*.md` file's
frontmatter `slug`, `name`, `lever`, `helps`, `hurts` fields only (not full bodies),
formatted as a YAML list. Full card bodies are NOT sent to the compiler.

### POST /api/ask/receipt
Body: `{ "ask": string, "body": string, "variables": [str], "checks": [...], "target_model": string, "n_inputs"?: number }`
Steps (all server-side, sequential):
1. Run INPUTGEN (04-PROMPTS §4) on `cheap_model` → `n_inputs` (default from config)
   sets of variable values.
2. For each input i: render `body` with vars → run on `target_model` (purpose
   `receipt_exec`); run the raw `ask` + "\n\n" + the same input values concatenated as
   `<name>:\n<value>` blocks → run on `target_model`.
3. Execute every check against both outputs (06-CHECKS).
→ `{ "inputs": [ { "vars": {...} } ], "rows": [ { "input_index": 0, "original_output": str, "compiled_output": str, "original_checks": [ { "id", "pass", "detail" } ], "compiled_checks": [...] } ], "totals": { "original_pass": n, "compiled_pass": n, "of": n }, "usd": number }`

### POST /api/artifacts
Body: `{ "draft": <compile draft>, "ask": str, "task_profile": {...}, "interrogation": [...], "target_model": str, "receipt"?: <receipt response> }`
Creates `workspace/artifacts/<slug>/artifact.md` (slug from title; on collision append
`-2`, `-3`, …). If `receipt` present and all compiled checks passed, set
`status: green`, `last_verified: now`, else `untested`. → `{ "slug": str }`

### GET /api/artifacts → `{ "artifacts": [ { slug, title, kind, version, tier, status, last_verified, target_model } ] }` sorted: regressed, untested, stale, green; then by `updated` desc.
### GET /api/artifacts/:slug → full parsed artifact `{ frontmatter, body }` + `{ "history_versions": [1,2,...] }`.
### PUT /api/artifacts/:slug — body `{ "body"?: str, "checks"?: [...], "title"?: str }`; snapshot to history, bump version, append changelog entry `{ summary: "Manual edit" }`.
### DELETE /api/artifacts/:slug → moves the directory to `workspace/.trash/<slug>-<ts>/`.

### POST /api/artifacts/:slug/run
Body: `{ "vars": { name: value }, "instruments"?: ["interpretation","consistency"] }`
Renders body with vars, runs on `target_model` (purpose `receipt_exec`), executes all
checks, updates `last_verified`/`status`, optionally runs instruments (07-INSTRUMENTS).
→ `{ "output": str, "checks": [...], "thinking": str|null, "instruments": { "interpretation"?: {...}, "consistency"?: {...} }, "usd": number }`

### POST /api/artifacts/:slug/refine
Body: `{ "feedback": string }` → runs REFINER (04-PROMPTS §7), snapshots history,
bumps version, changelog summary = first 80 chars of feedback.
→ `{ "slug", "version", "draft": {...} }` (same shape as compile draft).

### GET /api/runs?artifact=<slug>&limit=50 → `{ "runs": [ <run records newest first> ] }`

## 2. Later-phase endpoints
- P4: `GET /api/observe/sessions`, `GET /api/observe/sessions/:id`, `POST /api/observe/ingest` (08-COACH §1), gateway proxy (08-COACH §2).
- P5: `POST /api/coach/digest`, `GET /api/coach/digests`, queue: `GET /api/queue`, `POST /api/queue/:id/approve|reject`.
- P6: `GET/PUT /api/artifacts/:slug/suite`, `POST /api/artifacts/:slug/suite/run`, `POST /api/arena/bakeoff`, `POST /api/artifacts/:slug/ablate` (09-LAB).
- P8: `POST /api/models/:id/battery`, `POST /api/watch/scan` (10-CURRENCY).
Shapes are defined in those files.

## 3. Provider adapter — `providers.ts`

One exported function:
```ts
type LlmRequest = { model: ModelCard; system?: string; messages: {role:'user'|'assistant', content:string}[];
                    max_tokens: number; temperature?: number; tool?: {name:string, description:string, input_schema:object};
                    effort?: 'low'|'medium'|'high'|'max'; want_thinking?: boolean };
type LlmResult  = { text: string; tool_input?: object; thinking?: string;
                    usage: { input_tokens: number; output_tokens: number }; raw: object };
async function providerCall(req: LlmRequest): Promise<LlmResult>
```

### 3.1 provider = anthropic
POST `https://api.anthropic.com/v1/messages`
Headers: `x-api-key: $ANTHROPIC_API_KEY`, `anthropic-version: 2023-06-01`, `content-type: application/json`.
Body: `{ "model": card.api_model_id, "max_tokens", "system"?, "messages" }`.
Include `temperature` only if `card.params.temperature` is true AND the request sets it.
If `tool` present: add `"tools": [tool]`, `"tool_choice": { "type": "tool", "name": tool.name }`.
Thinking (2026 semantics — do NOT use budget_tokens, it 400s on current models):
- `card.reasoning.kind === "adaptive"`: if `want_thinking` or `effort` set, add
  `"thinking": { "type": "adaptive" }` and, when `effort` set,
  `"output_config": { "effort": effort }`; omit `temperature` when thinking is on.
- `card.reasoning.kind === "always-on"`: thinking is unconditional; send
  `"output_config": { "effort": effort }` when set; never send `temperature`.
- `card.reasoning.kind === "manual"`: if `want_thinking`, add
  `"thinking": { "type": "enabled", "budget_tokens": 4096 }` and omit `temperature`.
- `"none"`: no thinking fields.
Prefilled trailing assistant messages are never sent (removed on current Claude models).
Response: `text` = concat of `content[].text` where type=text; `tool_input` = first
`content[]` item with type=tool_use → its `.input`; `thinking` = concat of type=thinking
blocks' `.thinking`; usage from `.usage`.

### 3.2 provider = openrouter
POST `https://openrouter.ai/api/v1/chat/completions`,
header `Authorization: Bearer $OPENROUTER_API_KEY`.
Body: `{ "model": card.api_model_id, "max_tokens", "temperature"?, "messages": [ {role:"system"...}?, ... ] }`.
Tool use: `"tools": [{ "type":"function", "function": { name, description, "parameters": input_schema } }]`,
`"tool_choice": { "type":"function", "function": { "name": tool.name } }`;
`tool_input` = `JSON.parse(choices[0].message.tool_calls[0].function.arguments)`.
`thinking` = `choices[0].message.reasoning ?? null`. usage: `.usage.prompt_tokens/.completion_tokens`.
Effort: when `effort` set and `"reasoning_effort"` ∈ card's catalog `supported_parameters`,
send `"reasoning_effort": effort`. Send `temperature` only if `card.params.temperature`.

### 3.3 provider = openai — same wire shape as 3.2 against `https://api.openai.com/v1/chat/completions` with `$OPENAI_API_KEY`.

### 3.4 provider = local — same wire shape as 3.2 against `card.base_url + "/chat/completions"`.
No auth header. Covers Ollama (`http://localhost:11434/v1`), llama.cpp server, LM
Studio, vLLM — all OpenAI-compatible. Cost is always 0 (record usd_micro 0; token
usage still logged). `available` is always true for local cards; connection refusal
surfaces at call time as `provider_error` with message
`local runtime not reachable at <base_url> — is it running?`. Retries: single retry
after 1s (local failures are binary, not transient).

Retries: on network error or HTTP 429/5xx, retry up to 3 times with 1s/4s/10s waits;
then throw `provider_error` including status + first 200 chars of response body.

## 4. llmCall() — the only gateway

```ts
async function llmCall(purpose: Purpose, req: LlmRequest, opts?: { artifact?: string; bypassCache?: boolean }): Promise<LlmResult & { usd_micro: number; cached: boolean }>
```
Order of operations: (1) budget guard §5 → (2) cache lookup §6 (unless bypass or purpose
∈ {consistency, probe}) → (3) providerCall → (4) cost = tokens × card pricing, rounded
up to whole usd_micro → (5) append run record; update ledger; write cache file → return.

## 5. Budget guard
Before every uncached call: if `ledger.spent + estimate > monthly_cap_usd` →
throw `budget_exceeded` with message
`"Monthly cap of $<cap> reached ($<spent> spent). Raise it in settings."`.
Estimate = `(prompt_chars/4) × input price + max_tokens × output price`, where
prices come from the card's base `pricing` UNLESS the estimated prompt tokens
(`prompt_chars/4`) exceed a `pricing.tiers[].min_prompt_tokens` — then that
tier's prices apply (highest matching tier wins). Guard rails: if any resolved
price is negative (catalog sentinel), throw `bad_request` `"model has no usable
pricing"` — never let a negative estimate through the cap.

## 6. Cache — per 02-DATA §5. `promptText` for the key = system + "\n" + all message contents + (tool ? JSON.stringify(tool.input_schema) : "").

## 7. Static serving (production): non-`/api` GET → file from `app/dist`, else `dist/index.html`.
