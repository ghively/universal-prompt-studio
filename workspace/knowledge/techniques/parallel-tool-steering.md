---
slug: parallel-tool-steering
name: Parallel tool steering
lever: agentic
helps: agent prompts with many independent reads/searches; latency-sensitive agents
hurts: side-effecting toolsets — parallel writes to shared state are a correctness bug, not a latency tradeoff
---
Current models run independent tool calls in parallel by default and can be steered
to ~100%: "If you intend to call multiple tools and there are no dependencies between
the calls, make all independent calls in parallel. Never guess missing parameters —
if call B needs call A's output, run them sequentially."

**The asymmetry that matters: parallelize reads, serialize writes.** Independent
reads and searches are free wins; side-effecting calls (writes, sends, deletes) get
serialized or explicitly gated (approval-gates). Ordering dependencies and rate
limits are the other serialization triggers — state them explicitly instead of
hoping.

Parallelism is also a context decision: N parallel calls land N results in context
simultaneously — pair wide bursts with bounded/paginated tool responses, and keep
progress narration alive (legible-progress) or the burst is unsupervisable.
