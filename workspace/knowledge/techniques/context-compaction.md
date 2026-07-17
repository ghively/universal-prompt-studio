---
slug: context-compaction
name: Context compaction & the rot budget
lever: context
helps: long-running agents and any conversation approaching six figures of tokens
hurts: short tasks — pure overhead
---
**Context rot is measured, not folklore**: 18-model testing shows performance
degrades non-uniformly with input length even on trivial tasks — and focused
~300-token prompts beat the same information in a 113k-token dump across every
model family. Distractors compound; counterintuitively, coherent haystacks hurt
MORE than shuffled ones. Operating rule: budget effective context to a fraction
of the advertised window (practitioners run 25–65%), and treat "1M context" as
storage, not working memory.

Compaction practice (Anthropic/LangChain/OpenAI convergence): trigger by
threshold (~95% of budget); **clear stale tool results first** (lightest touch —
now a server-side API feature on Claude: compaction + context editing); when
summarizing, preserve decisions, open bugs, constraints, and artifact identifiers;
discard raw tool output post-extraction; tune for recall first, then precision.
The strong alternative: **fresh-start-from-files** — durable notes/todos/memory
files beat trusting an LLM summary. Match the tool to the shape: compaction for
conversational flow, note-files for milestone work, subagents (tens of thousands
of tokens in, 1–2k-token distilled summary out) for parallel research.
