---
slug: permission-rules-authoring
name: Permission rules as an artifact
lever: harness-artifacts
helps: designing settings.json allow/ask/deny rules as the deterministic layer under all prompts
hurts: n/a
---
Permission rules are the cheapest hook: declarative, no script, evaluated before
every tool call. Treat `.claude/settings.json` as a versioned artifact with a
design discipline:
- **Deny is policy, allow is friction-removal.** Deny rules encode invariants
  (`Bash(git push --force*)`, `Read(./.env)`); allow rules exist to stop prompt
  fatigue on known-safe calls (`Bash(npm test*)`). Ask rules mark the genuinely
  judgment-requiring middle.
- **Prefix patterns, not vibes**: `Tool(prefix *)` syntax; write the narrowest
  prefix that covers the intent. An over-broad allow is a standing grant every
  future prompt inherits.
- **Layering is the point**: managed > local > project > user. Put team
  invariants in checked-in project settings; personal conveniences in
  settings.local.json — never the reverse, or the invariant silently isn't one.
- Every deny rule deletes an instruction: "never touch .env" as a deny rule frees
  the CLAUDE.md line and cannot be argued with. Same logic as hook-vs-prompt,
  one level cheaper — reach for a hook only when the rule needs computation.
