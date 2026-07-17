# 06 — Check executor

`app/server/src/checks.ts` exports:
```ts
type CheckResult = { id: string; pass: boolean; detail: string; usd_micro: number };
async function runCheck(check: Check, output: string, vars: Record<string,string>): Promise<CheckResult>
async function runChecks(checks: Check[], output: string, vars: Record<string,string>): Promise<CheckResult[]>  // sequential
```

## 1. Check object
`{ id: string, type: string, config: object, source?: string }` — types below. Unknown
type at execution time → result `{ pass: false, detail: "unknown check type <t>" }`.

## 2. Deterministic types (usd_micro = 0)

### 2.1 json_valid `{}`
Extract candidate JSON: if the whole trimmed output parses, use it; else take the
substring from the first `{` or `[` to the last `}` or `]` and try that; else fail
with detail `"no parseable JSON found"`. Pass = parse succeeds. Detail on pass: `"parsed"`.

### 2.2 json_schema `{ schema }`
First run 2.1's extraction; fail if none. Then validate against the **restricted
dialect** — a recursive structure supporting exactly: `type: "object"` (+ `properties`,
`required`), `"array"` (+ `items`), `"string"`, `"number"`, `"boolean"`, and `enum:
[...]`. Implement `validateRestricted(schema, value): string[]` returning error paths
(≤ 40 lines; unit-tested). Pass = no errors. Detail: `"valid"` or first 3 error paths.

### 2.3 contains `{ value, case_sensitive?: false }`
Substring test (default case-insensitive). Detail: `"found"` / `"missing: <value>"`.

### 2.4 regex `{ pattern }`
`new RegExp(pattern, "s")` — construction error → fail with the error message.
Pass = `.test(output)`.

### 2.5 max_length `{ chars }`
Pass = `output.length <= chars`. Detail: `"<len>/<chars> chars"`.

## 3. LLM-judged types (run on `cheap_model` via llmCall purpose `check`)

### 3.1 quote_support `{ source_var }`
Source text = `vars[config.source_var]`; if the var is missing/empty → fail with detail
`"source variable <name> not provided"`. Run QUOTE_SUPPORT_JUDGE (04-PROMPTS §5.1).
Pass = `emit.pass`. Detail: `"<grounded>/<total> claims grounded"`, plus the first
ungrounded claim if failing.

### 3.2 rubric `{ criterion, min }`
Run RUBRIC_JUDGE (§5.2). Pass = `score >= min`. Detail: `"<score> — <reason>"`.

## 4. Labels (UI)
`json_valid` → `valid JSON`; `json_schema` → `matches schema`; `contains` →
`contains "<value>"`; `regex` → `matches pattern`; `max_length` → `≤ <chars> chars`;
`quote_support` → `quotes the <source_var>`; `rubric` → the criterion text (first 40 chars).

## 5. Cost attribution
LLM-judged check results carry their call's `usd_micro`; endpoint responses sum check
costs into their `usd` totals.
