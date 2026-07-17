# House practices — the living rubric

The compiler and critique passes cite this file. Update it as the field moves; every
edit is versioned by git. Cross-model truths live here; per-model quirks live in the
model cards, never duplicated. Last major revision: 2026-07-17, against Anthropic and
OpenAI current-generation guidance.

## What changed recently (read this first when returning after months away)

- **Effort replaced thinking budgets.** Adaptive thinking + an effort parameter
  (low→max) is the reasoning control on current Claude and GPT lines; manual
  budget_tokens errors on Claude 4.7+/Fable 5. In-prompt CoT on these models is
  harmful, not just wasteful.
- **Prefilled assistant turns are gone** on current Claude (400 error). Format
  forcing moved to native Structured Outputs / forced tool-use.
- **Lean beats elaborate.** Both major labs now measure *gains* from stripping
  scaffolding: repeated instructions, ALL-CAPS emphasis, anti-laziness padding all
  cause overtriggering on current models. State each rule once, calmly.
- **Verbosity is a parameter** (GPT text.verbosity; Claude verbosity via gateways),
  and models are concise by default — old "be brief" lines now overshoot.
- **Long context flipped**: documents at the TOP (in `<documents>` tags), query at
  the END — up to ~30% better on 20k+ inputs. Restate the ask after 100k+ tokens.
- **Agentic behavior is promptable surface area now**: action-vs-suggest defaults,
  parallel tool calling, persistence, subagent appetite, scope discipline, and
  context-window awareness all respond to one-block standing instructions.

## The order of operations for any prompt
1. **Say what you want** in one plain sentence before any structure. If you can't,
   the prompt isn't ready.
2. **Name the reader** (person or program) — it sets tone, detail, and format pressure.
3. **Make expectations enforceable**: schemas over adjectives, sentinels over hopes,
   quotes over trust. Every expectation that matters exists twice — once in the
   prompt, once as a check.
4. **Zero-shot first; examples when measured.** When needed: 3–5, realistic, diverse,
   in `<example>` tags.
5. **Place content deliberately**: stable system prefix first (cache), documents next,
   query last.
6. **Define failure**: what the model must do when it can't comply. Guessing is never
   the default.
7. **Match the model**: read the card. Effort level, structured-output mechanism,
   idiom (XML vs markdown), and whether CoT hurts are all card facts, not guesses.
8. **Give context for rules** ("read aloud by TTS, so never use ellipses") — current
   models generalize from the why.

## Standing rules
- Shorter beats longer at equal clarity; every sentence must earn its tokens.
- One prompt, one job. Stacked jobs get split into steps or chains.
- Negative instructions always carry a positive alternative.
- Calm imperatives; emphasis is a budget you spend on one rule at most.
- Never tune on vibes: a change is better only if the receipt or suite says so.
- A prompt that can't be tested isn't done — even one smoke check beats none.

## Anti-patterns (flag on sight)
- "You are a helpful assistant" — dead weight; scope the role to the reader or cut it.
- "Think step by step" aimed at an adaptive/always-thinking model — harmful; use effort.
- "CRITICAL: You MUST..." — causes overtriggering on current models.
- Repeated instructions and emphasis echoes — measured 10-15% quality cost.
- Prefill-style format forcing — removed; use structured outputs.
- Instructions buried mid-document; queries before long content.
- "Reply in JSON" without a schema when the target has native structured outputs.
- Blanket "be concise" on models that are already concise.
- Keyword walls ("high quality, professional, masterpiece") — noise on every modern model.
- Anti-laziness padding ("do not stop early", "if in doubt use the tool") ported from
  2024-era prompts — now causes overwork and overtriggering.
