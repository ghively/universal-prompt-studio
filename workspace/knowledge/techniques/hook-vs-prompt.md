---
slug: hook-vs-prompt
name: Hooks vs prompting
lever: harness-artifacts
helps: deciding whether a behavior belongs in instructions or in deterministic harness hooks
hurts: n/a — it's a boundary decision
---
A rule that must hold **100% of the time is not a prompting problem**. Prompted
instructions are probabilistic; harness hooks (pre/post tool-call scripts, exit
checks) are deterministic. "Never commit to main" as a CLAUDE.md line is a strong
suggestion; as a pre-commit hook it is physics.

Decision rule: prompt for *judgment* (style, approach, tradeoffs), hook for
*invariants* (blocked paths, required validations, formatting gates, secret
scanning). If you find yourself escalating instruction language on a rule the
agent keeps breaking ("NEVER, under ANY circumstances...") — stop; that rule wants
to be a hook. Hooks also free prompt tokens: every invariant moved to a hook is a
paragraph deleted from the prompt.
