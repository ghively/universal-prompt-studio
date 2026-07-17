---
slug: error-recovery-discipline
name: Error recovery & retry discipline
lever: agentic
helps: any tool-using agent — transient failure is the normal case, not the edge
hurts: nothing
---
Undirected agents thrash on failure: identical-retry loops, or worse, silent
route-arounds (skip the step, fabricate the output, mock the API). Give an
explicit ladder:
- **Retry** only plausibly-transient failures (timeouts, 429/5xx), with backoff
  and a hard cap: "never repeat an identical failing call more than twice."
- **Replan** on semantic failures (bad params, rejected input, wrong assumption):
  change the approach, not just the attempt — reread the exact error, re-verify
  preconditions, try a different route.
- **Escalate** when two distinct approaches fail or the failure implies an
  environment/permissions problem the agent cannot fix: report what was tried,
  the verbatim errors, and best-guess cause. Faking a result to keep momentum is
  the one forbidden move — name it.
Keep failed attempts in context (cache-and-append-discipline) — they are the
evidence the model learns from within the episode. And half of recovery is tool
design, not prompting: an error message that says what to call instead turns
recovery from search into instruction-following (tool-description-quality).
