# House practices — the living rubric

The compiler and critique passes cite this file. Update it as the field moves; every
edit is versioned by git. Cross-model truths live here; per-model quirks live in the
model cards, never duplicated.

## The order of operations for any prompt
1. **Say what you want** in one plain sentence before any structure. If you can't,
   the prompt isn't ready.
2. **Name the reader** (person or program) — it sets tone, detail, and format pressure.
3. **Make expectations enforceable**: schemas over adjectives, sentinels over hopes,
   quotes over trust. Every expectation that matters should exist twice — once in the
   prompt, once as a check.
4. **Show, don't describe**, when format or judgment matters: 1-3 realistic examples.
5. **Place content deliberately**: stable prefix first, long/variable content last,
   core ask restated after very long content.
6. **Define failure**: what the model must do when it can't comply. Guessing is never
   the default.
7. **Match the model**: read the card. Reasoning models get short direct prompts and
   an API thinking budget, not "think step by step". Claude gets XML sections; GPT and
   Gemini get markdown structure.

## Standing rules
- Shorter beats longer at equal clarity; every sentence must earn its tokens.
- One prompt, one job. Stacked jobs get split into steps or chains.
- Negative instructions always carry a positive alternative.
- Never tune on vibes: a change is better only if the receipt or suite says so.
- A prompt that can't be tested isn't done — even one smoke check beats none.

## Anti-patterns (flag on sight)
- "You are a helpful assistant" — dead weight; scope the role or cut it.
- Keyword walls ("high quality, professional, detailed, masterpiece") — noise on
  every modern model.
- "Think step by step" aimed at a reasoning model — actively harmful; see cards.
- Instructions buried mid-document — models attend to edges, not middles.
- "Reply in JSON" without a schema — begging, not engineering.
- Ten examples where two would do — cost without signal, and drift risk.
