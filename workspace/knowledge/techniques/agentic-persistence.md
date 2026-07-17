---
slug: agentic-persistence
name: Agentic persistence
lever: agentic
helps: long-running agent tasks, multi-step tool workflows, anything that must finish without a human mid-loop
hurts: interactive assistants where stopping to ask is the right behavior
---
Agents stop early by default. The persistence block raises completion rates on both
GPT and Claude lines: "Keep going until the user's query is completely resolved.
Only terminate when you are sure the problem is solved. Never stop at uncertainty —
research or deduce the answer and continue."

On models with **context awareness** (they track their remaining token budget), pair
it with harness truth: if compaction exists, say so ("your context will be compacted;
do not stop tasks early due to token budget concerns — save progress to memory as you
approach the limit"). Higher reasoning effort also increases tool-calling persistence.
