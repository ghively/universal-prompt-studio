# 08 — Observation & Coach (P4–P5)

## 1. Claude Code transcript ingestion (P4)

`POST /api/observe/ingest` body `{ "source_dir"?: string }` — default
`~/.claude/projects` (expand `~`). Also run automatically on server start and every 6h
(`setInterval`; skip silently if dir missing).

Mechanics: recursively find `*.jsonl`; for each file, session id = filename without
extension. Skip files already ingested with unchanged size (track in
`workspace/runs/observed/.ingest-state.json` as `{ [path]: { size, mtimeMs } }`).
Parse each line as JSON; ignore unparseable lines. Extract turns:
- a line with `type: "user"` and `message.content` → role `user`; content = if string,
  use it; if array, concat items where `item.type === "text"` → `item.text`. Skip if
  empty after that (tool results).
- a line with `type: "assistant"` and `message.content` array → role `assistant`,
  same text extraction.
- timestamps from `line.timestamp` when present; `cwd` from the first line carrying it;
  `model_hint` from the first assistant line's `message.model` if present.
Write per 02-DATA §9. Sessions with < 2 user turns are still stored.

`GET /api/observe/sessions?limit=50` → newest first:
`{ "sessions": [ { id, source, started, total_turns, first_ask: <first user turn, first 120 chars>, cwd } ] }`
`GET /api/observe/sessions/:id` → the full session object.

UI `/coach` (P4 portion): list of sessions, first_ask as row title (`--tx`), meta line
(source · date · N turns) in `--tx-faint`; click expands turns inline — user turns in
`--human`, assistant turns in `--tx-dim`, both 13.5px.

## 2. Gateway proxy (P4, second)

The server also listens on the same port for `POST /gateway/v1/messages` (Anthropic
shape) and `POST /gateway/v1/chat/completions` (OpenAI shape). Behavior: forward the
request verbatim to the real provider (base URL by shape; auth header passed through
from the client request), stream/relay the response bytes untouched, and append the
exchange to a gateway session file: session id = sha256(client `user-agent` +
UTC date)[0..16], source `gateway`; user turn = concatenated text content of the last
`user` message in the request; assistant turn = text of the response. Failures to
record must never fail the relay. Document for the user (README of workspace):
`export ANTHROPIC_BASE_URL=http://localhost:5680/gateway` etc.

## 3. Detectors (P5) — run by `POST /api/coach/digest` (and weekly `setInterval`, Mondays)

Input: all observed sessions from the last 7 days. Each detector emits candidate
patterns; each candidate is confirmed by FINDING_CONFIRM (04-PROMPTS §8.1) on
`utility_model` before it may appear in a digest. Max 5 findings per digest; order by
evidence count desc.

### D1 retry-loop
For each session, scan consecutive user turns (u1, u2) with no meaningful assistant
success between: candidate when `similarity(u1, u2) ≥ 0.55` where similarity =
Jaccard over lowercased word sets with stopwords removed (stopword list: the 50 most
common English words, hardcoded). Pattern text: `User rephrased the same request` +
both turns as evidence excerpts (first 300 chars each).

### D2 repeated-instruction
Across sessions: collect user-turn sentences (split on `.`/newline) of 4–25 words;
normalize (lowercase, strip punctuation); candidate when the same normalized sentence
(or Jaccard ≥ 0.7 pair) appears in ≥ 3 distinct sessions. Evidence: the sentence +
session ids.

### D3 underspecification
Heuristic: first user turn of a session with < 15 words followed by an assistant turn
containing a question mark in its first 200 chars → tally. Candidate when ≥ 30% of the
week's sessions match and count ≥ 4. Evidence: 3 example first-asks and the assistant
questions that followed. The dimension named in FINDING_CONFIRM's output is whatever
the model identifies from the excerpts.

### D4 spend-hotspot (gateway sessions only, or run-ledger)
Group the week's run records by `purpose`+`model`; candidate = top group if it is
> 50% of week spend and week spend > $1. Evidence: the numbers.

### Digest storage & queue
Digest file `workspace/journal/digest-<YYYY-WW>.md`: title `Coach · week <WW>`,
`from <n> observed sessions`, then each confirmed finding as `finding_md` + evidence
list. Each finding with `suggested_action: draft_claude_md` also runs FIX_DRAFT (§8.2)
and files a queue proposal (02-DATA §8) with `action.type: "none"` (the block is in
`body_md` for the user to copy or approve into a file of their choosing — P5 keeps
apply manual).

`GET /api/coach/digests` → list; UI `/coach`: latest digest rendered as sticky findings
(mockup `.sticky`: coach-wash, ±0.5° tilt, tag `Coach · week N`, finding bold, evidence
line linking to sessions, gold pill `Draft a fix` when applicable → shows the drafted
block with a `Copy` button and files/reveals the queue item).

### Detector discipline
Every digest footer: `Was this useful? [yes] [no]` per finding → `POST
/api/coach/feedback { digest, finding_index, useful }` appended to
`workspace/journal/coach-feedback.jsonl`. (Consumed by humans; no automation yet.)
