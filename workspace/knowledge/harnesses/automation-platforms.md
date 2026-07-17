---
slug: automation-platforms
name: Automation platforms (n8n, Zapier Agents)
memory_files: [none ambient — per-node system messages and knowledge sources]
artifact_kinds: [AI Agent node system messages, workflow expressions, agent instructions fields]
---
Workflow platforms embed LLM calls as nodes; the prompt surface is small but
templated, and the failure modes are pipeline-shaped:

**n8n**: the AI Agent node = chat model + a **System Message field** + tool
sub-nodes + optional memory sub-node (Simple Memory is in-instance and
non-persistent; Chat Memory Manager can inject system-type messages).
Expressions (`{{ $json.field }}`) interpolate upstream data into prompts — every
prompt is a template rendered per execution.

**Zapier Agents**: a single plain-language instructions field (auto-"enhanced" by
the platform — review what it actually sends), plus triggers, app connections,
and knowledge sources.

**Prompting implications**:
- These are headless CI-style prompts: no clarifying questions, no human retries —
  full brief, output contract, explicit failure behavior (ci-workflow-prompts
  applies verbatim).
- Interpolated fields are untrusted input arriving mid-prompt: quarantine them
  (untrusted-content-quarantine) — an email body flowing into `{{ $json.body }}`
  is a prompt-injection vector by construction.
- Structured output is the interface between nodes: schema + validate + a defined
  bad-output path (dead-letter, default, halt) or one malformed response breaks
  the pipeline downstream, silently.
- Memory defaults are weaker than they look (non-persistent buffers) — durable
  state belongs in the workflow's own storage, not the chat memory node.
