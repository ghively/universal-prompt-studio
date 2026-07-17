---
slug: subagent-delegation
name: Subagent delegation
lever: agentic
helps: orchestrator agents with subagent tools; steering over- or under-delegation
hurts: single-agent setups — irrelevant
---
Current frontier models orchestrate subagents natively and some now *over*-delegate
(spawning an agent where one grep would do). Steer with explicit criteria, not
enthusiasm: "Use subagents when tasks can run in parallel, need isolated context,
or are independent workstreams. Work directly for simple tasks, sequential
operations, and anything needing shared state across steps."

Each subagent's prompt is a fresh context: pass the *complete* brief (goal,
constraints, output contract) — it inherits nothing implicitly. Have it return
structured results, not prose logs.
