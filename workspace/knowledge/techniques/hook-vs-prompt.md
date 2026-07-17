---
slug: hook-vs-prompt
name: Hooks vs prompting
lever: harness-artifacts
helps: deciding whether a behavior belongs in instructions or in deterministic harness hooks
hurts: n/a — it's a boundary decision
---
A rule that must hold **100% of the time is not a prompting problem**. Prompted
instructions are probabilistic; hooks fire deterministically on harness events
(PreToolUse, PostToolUse, UserPromptSubmit, Stop, SessionStart, ...). "Never
commit to main" as a CLAUDE.md line is a strong suggestion; as a PreToolUse deny
(or a permissions.deny rule) it is physics. (A git pre-commit hook is a different
layer — the agent can be asked to bypass it with `--no-verify` unless that too is
blocked.)

Decision rule: prompt for *judgment* (style, approach, tradeoffs), hook for
*invariants* (blocked paths, required validations, formatting gates, secret
scanning). If you find yourself escalating instruction language on a rule the
agent keeps breaking ("NEVER, under ANY circumstances...") — stop; that rule wants
to be a hook. Hooks also free prompt tokens: every invariant moved to a hook is a
paragraph deleted from the prompt.

Two refinements to the binary:
- **The middle tier is context injection**: hooks can *say things to the model*
  deterministically (`additionalContext` at the right event) — often the right
  fix for "the agent keeps forgetting X"; cheaper than blocking, stronger than
  hoping.
- **The trigger is deterministic; the handler need not be**: handlers can be
  scripts (deterministic) or LLM judges/agents — a probabilistic gate at a
  deterministic moment. The dichotomy holds fully for script handlers.
The canonical cure for prompt escalation on "always run tests": a Stop-gate hook
that blocks completion until the suite is green, feeding the failure back.
Ladder of cost: permission rule (declarative) → hook (computed) → prompt
(judgment). See permission-rules-authoring and hook-recipes.
