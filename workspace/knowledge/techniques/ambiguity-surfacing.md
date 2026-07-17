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

**Agent-era gating**: license asking on expected information gain and action
IRREVERSIBILITY — ask before destructive/irreversible tool actions, proceed with
stated assumptions otherwise. Question QUALITY is the real bottleneck: licensed
models still ask low-value questions because they can't assess their own
knowledge boundaries — constrain to "up to 2 questions, only where the answer
changes your approach."
