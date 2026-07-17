---
slug: investigate-before-answering
name: Investigate before answering
lever: grounding
helps: agents with read access (code, files, search) answering questions about that material
hurts: nothing where tools exist; irrelevant without them
---
The agent-era version of quote-then-answer: forbid speculation about material the
model can open. "Never speculate about code you have not opened. If the user
references a specific file, read it before answering. Investigate relevant files
BEFORE answering questions about them — give grounded answers only."

**Mechanism**: hallucination in agentic settings is usually skipped retrieval, not
imagination — the model answers from priors because looking costs a tool call. Making
investigation mandatory converts guesses into reads.
