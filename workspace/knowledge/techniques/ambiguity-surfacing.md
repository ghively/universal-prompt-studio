---
slug: ambiguity-surfacing
name: Ambiguity surfacing
lever: verification
helps: interactive use; underspecified asks; high blast-radius tasks (code changes, deletions)
hurts: batch pipelines (nothing is there to answer)
---
License the model to ask: "If any requirement is ambiguous, ask up to 2 clarifying
questions before doing the work; otherwise proceed." For batch prompts, redirect the
same instinct into assumptions: "state any assumptions you made at the end."

**Mechanism**: models default to guessing because asking feels like failure to them;
explicit permission flips the default on exactly the inputs where guessing is most
expensive.
