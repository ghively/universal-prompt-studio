---
slug: langgraph
name: LangGraph / LangChain 1.x (deep dive)
memory_files: [none ambient — state schemas, checkpointers, Store, and prompt files you own]
artifact_kinds: [node prompt templates, state schemas, middleware, handoff tools, LangSmith Prompt Hub entries]
---
**Current surface (verified July 2026)**: langgraph 1.2.x; LangChain 1.x
`create_agent` (built ON the LangGraph runtime, with middleware) is the recommended
high-level path; raw `StateGraph` for custom orchestration; **`deepagents`** is the
opinionated batteries-included harness (todos tool, virtual filesystem, subagents —
LangChain's productized Claude-Code pattern). `config_schema` is deprecated →
`context_schema` + `Runtime`.

**How context actually flows**: nothing reaches the LLM automatically — each node
builds its own call from the state fields it chooses to read. State = typed schema
+ **reducers** per key (`add_messages` appends; unbounded by default). The official
taxonomy: model context (per-call), tool context (state/store reads-writes),
lifecycle context (between-step summarization/guardrails) — controlled via
middleware hooks (`@dynamic_prompt`, `@before_model`, `wrap_tool_call`...).

**Memory**: checkpointer = short-term (full state saved after every node, per
thread_id; `InMemorySaver` is dev-only — Postgres/Sqlite in prod). **Store API** =
long-term cross-thread JSON with semantic search. Growth control:
`SummarizationMiddleware(trigger=("tokens",N), keep=("messages",M))`,
`trim_messages`, `RemoveMessage`. After ANY message surgery, ensure provider-valid
history (orphaned tool results / dropped system messages are the classic breakage).

**Multi-agent**: supervisor (`langgraph-supervisor`) vs swarm
(`langgraph-swarm`); handoffs via `Command(goto=..., update=..., graph=PARENT)`;
`Send` for fan-out. **What crosses a handoff is a deliberate choice** — full
history, last-N, or a summary; the prebuilt default passes everything.

**Measured failure modes**: state bloat (append-only messages + every-node full
reads → 300-800ms checkpoint writes at 500KB state); checkpoint serialization
overhead (measured 6.79x storage on a 16-turn agent — issue #7714); >60% of
production incidents attributed to state management (LangChain's own 2026 report,
secondhand). Fixes that converged: bound every list-typed key with a reducer,
rolling-summary field, artifacts in Store/filesystem with pointers in state.

**Prompts in code**: two respectable modes — template files in git (evals in CI
gate merges) or LangSmith Prompt Hub (commit hashes, staging/production tags,
owner ACLs; pin hashes in prod, promote by moving tags). Never pin prod to
`latest`.
