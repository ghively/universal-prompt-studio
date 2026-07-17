---
slug: openai-agents-sdk
name: OpenAI Agents SDK
memory_files: [none ambient — Sessions you configure]
artifact_kinds: [Agent instructions (string or callable), handoff configs, guardrails, session backends, sandbox/manifest configs]
---
Primitives: Agent, Runner, Tools, **Handoffs**, Guardrails, **Sessions** (+ tracing).

**The #1 context-engineering lever is the handoff default**: handoffs are exposed
as `transfer_to_<agent>` tools and the receiving agent sees the ENTIRE prior
conversation unless you filter. Multi-hop handoffs flood specialists with
irrelevant tool noise — use `input_filter` (receives HandoffInputData; stock:
`remove_all_tools`), `nest_handoff_history`, and `input_type` for small
structured handoff metadata (reason/priority/summary). Prepend
`RECOMMENDED_PROMPT_PREFIX` to the system prompts of any agent involved in
handoffs.

**Sessions** auto-prepend history and auto-append results; backends now include
SQLite/Postgres(SQLAlchemy)/Redis/Mongo/Dapr/Encrypted and
**`OpenAIResponsesCompactionSession`** (automatic server-side compaction with
thresholds). `SessionSettings(limit=N)` bounds history; `session_input_callback`
customizes merging. Guardrails run in parallel with the agent and fast-fail on
tripwire.

**The April 2026 release changed the shape**: native **sandbox execution**
(built-in Blaxel/Cloudflare/Daytona/E2B/Modal/Runloop/Vercel or BYO), the
**Manifest** workspace abstraction (S3/GCS/Azure/R2), and a **model-native
harness** with Codex-like filesystem tools and `apply_patch` — prompts written
for tool-less agents under-specify against it; write for an agent that can read,
patch, and execute. **Temporal integration is GA** (each agent invocation as a
durable Activity) — the long-running-agent persistence story lives there, not in
prompt tricks. AgentKit (Agent Builder/ChatKit/Evals) is the visual platform
layer; this SDK remains the code-first path.

Prompt structure is a plain `instructions` string or callable per Agent — no
built-in registry; pair with promptfoo/Braintrust/LangSmith for versioned evals.
