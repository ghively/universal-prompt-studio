---
slug: worked-examples
name: Worked examples (modeling a reasoning style)
lever: examples
helps: tasks where HOW to decide matters more than output format — rubric application, triage, judgment calls with named criteria
hurts: adaptive/always-thinking models when the worked steps conflict with native reasoning (anchors it); simple lookups
---
A worked example shows the decision process, not just input→output: a short labeled
reasoning trace (e.g. a <thinking> block) inside the example showing which factors
were weighed and in what order. Current models generalize the demonstrated *style*
into their own thinking rather than parroting the steps.

**Distinct from CoT prompting**: you are not asking the model to think (it already
does); you are showing what good thinking looks like for this task. Model-generated
traces, checked and pruned, often work as well as hand-written ones.

**Rules**: keep traces short and criterion-shaped ("checked source for the date —
absent → flag"), never narrative; never include a trace whose conclusion you
wouldn't defend — the mapping is learned, errors and all. On reasoning models,
receipt-check that the worked example helps; described criteria alone often
suffice and cost less.
