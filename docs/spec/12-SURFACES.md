# 12 — Surfaces: MCP server, CLI, CI (P7)

## 1. MCP server
`app/server/src/mcp.ts` — a **stdio** MCP server started by `npm run mcp`
(add script: `"mcp": "tsx server/src/mcp.ts"`). Implement the protocol by hand over
stdio JSON-RPC (initialize, tools/list, tools/call) — no SDK dependency; the subset is
~120 lines. It calls the same workspace/llm functions as the HTTP server (import, not
HTTP).

Tools:
1. `search_artifacts { query: string, kind?: string }` → up to 10 of
   `{ slug, title, kind, status, target_model }`; match = case-insensitive substring of
   title/slug/original_ask, ranked by status order then recency.
2. `render_prompt { slug: string, vars: object }` → `{ body: string }` (rendered;
   error if a variable is missing).
3. `model_brief { id: string }` → `{ brief: string }` — the card's label, idiom,
   prompting_notes, sampling notes, plus measured metrics if present, as markdown.
4. `log_outcome { slug: string, worked: boolean, note?: string }` → appends
   `{ at, slug, worked, note }` to `workspace/journal/field-outcomes.jsonl`; returns
   `{ ok: true }`. Artifacts with ≥ 3 consecutive `worked: false` get status
   `regressed` (checked at artifact read time).

Register for Claude Code with:
`claude mcp add prompt-studio -- npx tsx <abs path>/app/server/src/mcp.ts`

## 2. CLI
`app/server/src/cli.ts`, script `"cli": "tsx server/src/cli.ts"`. Commands (parse
`process.argv` by hand):
- `ask "<text>" [--model id] [--yes]` — full ask pipeline in the terminal; `--yes`
  auto-skips interrogation (empty answers). Prints the compiled prompt, annotations as
  indented notes, then asks `Run receipt? (y/N)`; prints tally. Saves on `Save? (y/N)`.
- `run <slug> --var name=value ...` — run artifact, print output + check lines.
- `eval <slug> [--json]` — run suite; `--json` prints the response JSON; exit code 0
  if all cases pass else 1.
- `eval --all [--json]` — every artifact with a suite; exit 1 if any fail.
- `bench <model-id>` — run the probe battery, print dimension table.
Output styling: plain text, check marks `✓`/`✕`, no colors (CI-safe).

## 3. CI mode
`eval --all --json` is the CI entry point. Document in workspace README:
a GitHub Action step `cd app && npm ci && npm run cli -- eval --all --json > eval.json`
with `ANTHROPIC_API_KEY` from secrets; a second step posts `eval.json` totals as a PR
comment (left to the consuming repo; not built here).
