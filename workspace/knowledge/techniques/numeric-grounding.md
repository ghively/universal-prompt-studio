---
slug: numeric-grounding
name: Numeric grounding
lever: grounding
helps: any task involving arithmetic, aggregation, unit conversion, or financial/statistical claims
hurts: nothing where a code tool exists; without tools, adds a verification pass to budget for
---
Models pattern-match arithmetic instead of executing it — fluent, precise-looking,
wrong numbers are a signature hallucination class, and precision decays with digit
count and chained operations. Controls:
- **Route computation to code**: "use the code tool for any calculation; never do
  multi-step arithmetic in your head." This is the one grounding fix that is nearly
  free wherever a sandbox exists.
- **No freehand derived numbers**: totals, percentages, deltas must come either
  quoted from the source or produced by executed code — the same rule
  quote-then-answer applies to facts, applied to figures.
- **Split copied from computed** in extraction contracts: copied numbers get the
  quote check; computed numbers get recomputed by the harness (re-add the column).
- **Verify the setup, not just the sum**: tool use shifts errors rather than
  erasing them — fewer arithmetic mistakes, more wrong-formula and wrong-operand
  mistakes. Ask for the expression and inputs, not only the result.

**Mechanism**: next-token prediction approximates arithmetic distributionally.
Grounding a number means giving it provenance — quoted from source or emitted by a
deterministic tool; anything else is generated, however confident it looks.
