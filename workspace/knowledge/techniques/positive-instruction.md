---
slug: positive-instruction
name: Positive instruction
lever: clarity
helps: any prompt containing "don't", "avoid", "never" without an alternative
hurts: nothing
---
Tell the model what to **do**, not only what to avoid. Negations leave a hole where
the behavior should be; models fill holes badly, and mentioning the forbidden thing
makes it more salient.

**Mechanism**: an instruction is a target. "Don't be verbose" has no target; "answer
in at most 3 sentences" does.

**Before**: "Don't make up details."
**After**: "Only state facts you can quote from the transcript; if a detail is
missing, write UNKNOWN."
