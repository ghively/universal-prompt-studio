---
slug: framework-orchestrators
name: Framework orchestrators (LangGraph, OpenAI Agents SDK, CrewAI, ...)
memory_files: [none — prompts live in your code]
artifact_kinds: [node/agent prompt templates, state schemas, handoff messages, tool definitions]
---
**What's distinct**: no ambient memory files — every prompt is code you wrote, per
node/agent, and the "conversation" is a state object your graph passes around. The
harness impact is total: context is exactly what your code assembles each step.

**Prompting implications**:
- Each node prompt is a fresh context: pass the complete brief (goal, constraints,
  output contract) every time; nothing is inherited implicitly.
- Inter-node handoffs are prompts too — structured state schemas beat prose
  summaries between steps (the receiving node can't ask clarifying questions).
- Version node prompts like this studio versions artifacts: template files, not
  string literals scattered in code — otherwise they're untestable and invisible.
- Judge/router nodes are judges: judge-design applies (anchored scales,
  position-swapping for A/B routing).
