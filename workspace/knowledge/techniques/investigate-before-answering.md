---
slug: investigate-before-answering
name: Investigate before answering
lever: grounding
helps: agents with read access (code, files, search) answering questions about that material
hurts: quick-answer paths — a blanket "always read first" mandate is anti-laziness padding and causes overwork (see practices)
---
The agent-era version of quote-then-answer: forbid speculation about material the
model can open. "Never speculate about code you have not opened. If the user
references a specific file, read it before answering. Investigate relevant files
BEFORE answering questions about them — give grounded answers only."

**Mechanism**: hallucination in agentic settings is usually skipped retrieval, not
imagination — the model answers from priors because looking costs a tool call. Making
investigation mandatory converts guesses into reads.

**Scope it**: reads are mandatory for material the user *names*; elsewhere,
investigation proportional to the claim — "if in doubt, always use the tool" is the
measured overtriggering anti-pattern. Where web search is a tool, the same rule
covers recency-sensitive facts (see temporal-grounding). Two disciplines make it
checkable: **cite your reads** (file:line references are the agentic quote — they
turn "I looked" into a verifiable claim), and a closing pass that every asserted
claim traces to something actually opened (see self-check-pass).
