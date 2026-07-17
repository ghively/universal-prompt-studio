---
slug: termination-conditions
name: Termination conditions
lever: agentic
helps: any autonomous loop, and every prompt that contains a persistence block; cost- or time-capped agents
hurts: nothing — undefined "done" is itself the failure mode
---
Persistence without a definition of done produces the opposite failure: overwork,
frantic re-verification, tool-call burn. Current guidance from both labs is
symmetric: define the destination, set the stopping conditions, get out of the way.
Every autonomous prompt needs three explicit exits:
- **Success** — a *checkable* done-condition ("all entries in tests.json pass and
  lint is green"), verified before declaring victory, never asserted. "Until the
  problem is solved" with no checkable "solved" is an infinite loop with a meter.
- **Budget** — tool-call / iteration caps ("maximum 5 context-gathering calls;
  prioritize the most critical first") with defined at-cap behavior: report the
  best result so far and what remains, don't silently truncate.
- **Futility** — hand off to the escalate arm of error-recovery-discipline: two
  distinct approaches failed, or the blocker is outside the agent's permissions.
Pair every agentic-persistence block with all three. On long-horizon work the
success condition lives in a state file (state-and-compaction), so "done" survives
window resets instead of being renegotiated by each fresh context.
