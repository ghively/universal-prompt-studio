---
slug: message-role-semantics
name: Message role semantics
lever: structure
helps: any multi-role or agentic prompt; anything targeting OpenAI-lineage models
hurts: nothing
---
Roles are an authority ladder, not labels. OpenAI's Model Spec fixes the conflict
order: **platform/system > developer > user > tool** — and since o1, "system" is
renamed **developer** (auto-aliased). Anthropic keeps a top-level `system`
parameter. Models are trained to resolve conflicts by rank, and **tool results
plus quoted data carry no authority at all** — the trained backbone of injection
resistance.

**Practice**: put rules at the highest rank you control; treat the tool role as a
data channel and never echo instructions through it; on the Responses API,
`instructions` is per-call developer text that does not persist across chained
turns. Porting Anthropic → OpenAI: system-param content becomes a developer
message.
