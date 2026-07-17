---
slug: plan-then-execute
name: Plan-then-execute
lever: reasoning
helps: multi-constraint generation; long-horizon agentic work (as the TWO-CALL approval variant); non-reasoning models on complex tasks
hurts: reasoning models — but ONLY the in-prompt "write a plan first" ritual; the two-call variant is provider-recommended for exactly these models
---
Two variants with opposite 2026 verdicts:

**In-prompt planning ritual** ("first write a plan, then execute") — redundant on
reasoning models; they plan natively while thinking. Skip it.

**Two-call plan → approve → execute** — alive and provider-endorsed: Claude Code
ships a plan mode; outcome/rubric loops formalize it; the principle is "full task
specification up front in one well-specified turn" (beats progressively-revealed
multi-turn on both tokens and quality). A separate plan call is cheap insurance
on expensive execution, makes the interpretation visible for approval (pairs with
problem-reformulation), and the approved plan becomes a stable cached prefix.

**The failure mode it prevents**: committing an expensive autonomous run to a
misread. The failure mode it can cause: overplanning on unambiguous tasks — pair
with "when you have enough information to act, act."
