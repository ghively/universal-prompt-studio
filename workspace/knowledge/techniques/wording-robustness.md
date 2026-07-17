---
slug: wording-robustness
name: Wording robustness
lever: clarity
helps: interpreting eval results; prompts assembled from templates or user input; deciding whether a "winning" variant is real
hurts: nothing — but don't use sensitivity as an excuse to skip measurement; measure the spread instead
---
Semantically identical prompts are not behaviorally identical. Changing only
separators, casing, or phrasing — meaning held constant — moved accuracy by up to
76 points in controlled studies (FormatSpread), and the sensitivity survives
scale and instruction tuning. Typos degrade instruction-following; politeness
level alone shifts benchmark accuracy a few points.

**Consequences**: proofread prompts like production code — misspellings are not
free. Never crown a variant on one phrasing; a real improvement survives
paraphrase — test the winner reworded before believing the delta. When templating
user text into prompts, normalize what you can; the model inherits the user's
noise. Single-run, single-phrasing comparisons are anecdotes.
