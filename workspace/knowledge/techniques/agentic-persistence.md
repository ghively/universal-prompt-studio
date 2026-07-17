---
slug: agentic-persistence
name: Agentic persistence
lever: agentic
helps: long-running agent tasks, multi-step tool workflows, anything that must finish without a human mid-loop
hurts: interactive assistants where stopping to ask is right; any prompt with no termination condition — persistence without a definition of done produces overwork
---
Agents stop early by default — but the fix is now effort-scoped, not unconditional.
The persistence block ("Keep going until the user's query is completely resolved.
Only terminate when you are sure the problem is solved. Never stop at uncertainty —
research or deduce the answer and continue.") is critical at LOW reasoning effort,
where premature termination is the documented failure; at high effort it is largely
redundant, and the documented failure flips to **overthinking** — endless
double-checking when the prompt has no clear definition of done.

Two pairings make it safe:
- **A checkable done-condition**: "solved" must be *verified* (run the test, take
  the screenshot), never asserted. Every persistence block needs the three exits —
  success, budget, futility (see termination-conditions).
- On models with **context awareness** (they track their remaining token budget),
  harness truth: if compaction exists, say so ("your context will be compacted; do
  not stop tasks early due to token budget concerns — save progress to memory as
  you approach the limit").
Higher reasoning effort also increases tool-calling persistence on its own —
another reason the block should shrink as effort rises.
