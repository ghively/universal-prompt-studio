---
slug: subagent-delegation
name: Subagent delegation
lever: agentic
helps: orchestrator agents with subagent tools; steering over- or under-delegation
hurts: single-agent setups — irrelevant
---
Current frontier models orchestrate subagents natively and some now *over*-delegate
(spawning an agent where one grep would do). Steer with explicit criteria AND
explicit scale: "Use subagents when tasks can run in parallel, need isolated
context, or are independent workstreams. Work directly for simple tasks, sequential
operations, and anything needing shared state across steps. Scale to the ask:
1 agent for simple fact-finding, 2-4 for comparisons, 10+ only for complex
research." Without embedded counts, orchestrators over-scale — and multi-agent
runs cost roughly 15x single-chat tokens, so delegation must clear a value bar.

Each subagent's prompt is a fresh context: pass the *complete* brief — objective,
constraints, output contract, tool/source guidance, AND task boundaries (vague
boundaries make subagents duplicate each other's searches, the documented failure
mode). Don't rely on implicit inheritance: harness subagents may auto-load memory
files and tools (check the harness card), but they never see the conversation or
each other — the orchestrator owns deduplication and conflict reconciliation.
Have each return structured results, not prose logs.
