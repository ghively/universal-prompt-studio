---
slug: conflict-free-instructions
name: Conflict-free instructions
lever: clarity
helps: prompts grown by accretion; migrated prompts; any rule set with exceptions; reasoning models
hurts: nothing — but resolving a conflict means choosing, and someone must own the choice
---
Two rules that can collide are worse than one rule missing. Current reasoning
models don't pick a side and move on — they burn reasoning tokens trying to
reconcile the irreconcilable, and output quality drops with latency up. OpenAI's
current guidance is blunt: conflicting rules create more instability than missing
detail. Instruction-hierarchy benchmarks agree: even frontier models score under
50% at resolving multi-tier conflicts by source priority.

**Practice**: audit for collisions before adding any rule — every "always"
against every "never", every default against every exception. Resolve explicitly:
state precedence ("when X and Y conflict, X wins") or rewrite the general rule to
carve out the exception. A conflict the author didn't notice becomes a coin-flip
the model decides differently per run.
