---
slug: framework-orchestrators
name: Other orchestrators (MS Agent Framework, CrewAI, Google ADK)
memory_files: [none ambient — per-framework state/session constructs]
artifact_kinds: [agent configs, task definitions, state schemas]
---
**Microsoft Agent Framework 1.0** (April 2026, Python + .NET, LTS) is the official
successor merging AutoGen + Semantic Kernel — **both predecessors are in
maintenance mode**; any guidance recommending "AutoGen for multi-agent" is stale.
Start new Microsoft-stack projects here.

**CrewAI 1.x**: role/goal/backstory concatenate into the system prompt — but the
framework **silently injects additional default instructions**; production teams
must read the customizing-prompts docs to see and override the real assembled
prompt. The highest-leverage field is the Task's **`expected_output`** (vague
expected_output is the top reliability complaint); there's no evidence
role/goal/backstory beats simpler persona prompting — its value is standardized
structure. Failures come from agents trusting hallucinated intermediate outputs,
not weak backstories: check inter-task outputs.

**Google ADK**: Session (event log) + State (key-value scratchpad) + Memory
(Vertex AI Memory Bank). Signature quirk: **state templating into instructions** —
`instruction="...{key}..."` auto-substitutes session state before the call; state
key prefixes scope persistence (`user:` cross-session, `app:` global, `temp:`
invocation-only). Prompt = function of state, explicitly.

Universal rules regardless of framework: every node/agent prompt is a fresh
context (pass the complete brief); handoffs are prompts (structured state beats
prose summaries); prompts in code are untestable until extracted into versioned,
eval-gated templates.
