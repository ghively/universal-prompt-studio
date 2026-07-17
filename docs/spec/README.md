# Prompt Studio — Build Specification (index)

This spec is written so a coding agent can build the entire product **without making
design decisions**. Every format, endpoint, prompt, token value, and behavior is stated
exactly. Where content requires judgment (LLM meta-prompts, model cards, technique
cards), it has already been authored and must be used verbatim.

## Files

| File | Contents |
|---|---|
| `01-STACK.md` | Tech stack (pinned), directory layout, verbatim config files, npm scripts, env |
| `02-DATA.md` | Every workspace file format: artifacts, model cards, runs, ledger, queue, observed |
| `03-API.md` | Every server endpoint (request/response shapes), provider adapter, cache, budget guard |
| `04-PROMPTS.md` | Every LLM prompt the app sends, **verbatim**, with structured-output schemas |
| `05-UI.md` | Design tokens (exact values), routes, components, states, accessibility |
| `06-CHECKS.md` | Check executor: semantics of every check type, deterministic and LLM-judged |
| `07-INSTRUMENTS.md` | Interpretation report, consistency probe, refine loop |
| `08-COACH.md` | Observation adapters (transcripts, gateway proxy), detectors, weekly digest |
| `09-LAB.md` | Suites, graders + anchors, pairwise judging, arena, ablation, promotion |
| `10-CURRENCY.md` | Model watcher, field scan, probe battery, regression sentinel, fix candidates |
| `11-AGENTS.md` | Skill/agent wind tunnel: scenarios, headless runner, 3-axis scoring |
| `12-SURFACES.md` | MCP server, CLI, CI mode |
| `13-ACCEPTANCE.md` | **The build checklist.** Phases P0–P9, each item with a concrete test |

## The goal (one paragraph)

Build a local web app + server ("Prompt Studio") where the user types what they need
done in plain language; the app asks 2–4 clarifying questions; compiles one prompt
targeted at a chosen model using the model's card and the technique cards; renders it
with every section annotated (which technique, why, and why for this model); proves it
beats the user's raw ask by running both on generated inputs and scoring them with
checks derived from the user's answers ("the receipt"); and saves it as a versioned file
with health tracked over time. Later phases add: instruments that expose the model's
reaction; observation of the user's real AI usage with weekly coaching; a lab for
curated test suites and bake-offs; self-updating model knowledge; a harness for testing
agent skills; and MCP/CLI surfaces. The full product definition is `../PRODUCT.md`;
this spec is its mechanical translation.

## Global conventions (apply everywhere)

1. **Language/runtime**: TypeScript everywhere, Node ≥ 22, ESM (`"type": "module"`).
2. **IDs**: `crypto.randomUUID()`. **Timestamps**: ISO 8601 UTC strings (`new Date().toISOString()`).
3. **Hashing**: `sha256` hex via `node:crypto`; helper `hash(s: string): string`.
4. **Money**: numbers in USD with 6 decimal places max; never floats in ledger math —
   store microdollars as integers (`usd_micro`), render as USD in UI.
5. **Errors (server)**: every non-2xx response body is exactly
   `{ "error": { "code": string, "message": string } }`. Codes used:
   `bad_request`, `not_found`, `conflict`, `budget_exceeded`, `provider_error`, `internal`.
6. **Validation**: all request bodies and all workspace files read from disk are
   validated with `zod` schemas defined in `app/server/src/schemas.ts`. Invalid file →
   respond `internal` with the file path in `message`; never crash the process.
7. **No streaming** in P0–P3 (responses render when complete). SSE streaming is an
   optional enhancement listed in P3 as a stretch item.
8. **Every LLM call** goes through one function `llmCall()` (03-API §4) which enforces
   the budget guard, writes the run record, and consults the cache. No direct fetches
   to providers anywhere else in the codebase.
9. **Workspace path**: resolved from env `WORKSPACE_DIR`, default `../workspace`
   relative to `app/`. All reads/writes go through `app/server/src/workspace.ts`.
10. **Prompt templates**: stored in `app/server/src/prompts.ts` as exported string
    constants copied verbatim from `04-PROMPTS.md`. Template slots use `{{SLOT_NAME}}`
    and are replaced with simple string substitution.
11. **Structured LLM output**: always via Anthropic tool-use (04-PROMPTS §0). Never
    parse JSON out of freeform text except where a spec section explicitly says so.
12. **Tests**: `vitest`. Unit tests colocated as `*.test.ts`. Acceptance tests are the
    commands in `13-ACCEPTANCE.md`.
13. **Do not add dependencies** beyond those listed in 01-STACK §2 without a
    `// SPEC-AMBIGUITY:` comment explaining why.
