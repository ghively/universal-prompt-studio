---
slug: parallel-tool-steering
name: Parallel tool steering
lever: agentic
helps: agent prompts with many independent reads/searches; latency-sensitive agents
hurts: workflows with hidden ordering dependencies (be explicit about them instead)
---
Current models run independent tool calls in parallel by default and can be steered
to ~100%: "If you intend to call multiple tools and there are no dependencies between
the calls, make all independent calls in parallel. Never guess missing parameters —
if call B needs call A's output, run them sequentially."

Steer the other way ("execute operations sequentially") when parallel bursts
overwhelm rate limits or shared systems.
