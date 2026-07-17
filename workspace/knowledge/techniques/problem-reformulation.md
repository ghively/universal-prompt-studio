---
slug: problem-reformulation
name: Problem reformulation (rephrase before solving)
lever: reasoning
helps: ambiguous, underspecified, or oddly-phrased questions; user-authored queries
hurts: already-precise specs (adds tokens for nothing)
---
Have the model restate the question — resolving ambiguity, expanding
abbreviations, making implicit constraints explicit — before answering, or as a
cheap first call feeding the real one: "Rephrase and expand the question, then
respond."

**Mechanism**: most wrong answers to ambiguous questions are right answers to the
wrong reading. Reformulation surfaces the interpretation so it can be checked; a
rephrase from a strong model measurably transfers to weaker executors (RaR,
Self-Polish).

**Reasoning-era caveat** [evidence base is 2023-24, pre-reasoning models — they
already reformulate silently while thinking]: current value concentrates in (a)
making the interpretation VISIBLE for approval (pairs with plan-then-execute —
and it is literally this studio's interrogation step) and (b) low-effort/fast
paths where no thinking happens. Measure before adopting on high-effort routes.
