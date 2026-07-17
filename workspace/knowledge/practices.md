# House practices — the living rubric

The compiler and critique passes cite this file. Update it as the field moves; every
edit is versioned by git. Cross-model truths live here; per-model quirks live in the
model cards, never duplicated. Last major revision: 2026-07-17, against Anthropic and
OpenAI current-generation guidance.

## What changed recently (read this first when returning after months away)

- **Effort replaced thinking budgets.** Adaptive thinking + an effort parameter
  (Anthropic: low|medium|high|xhigh|max, DEFAULT high; OpenAI: per-model range) is
  the reasoning control; budget_tokens hard-errors on Claude 4.7+/Sonnet 5/Fable 5.
  Calibrate by SWEEP on your eval (non-monotonic cost curve), never by vibes.
  In-prompt CoT on adaptive/always-on models is harmful; interleaved thinking is
  automatic; thinking display now DEFAULTS to omitted (opt into summaries); task
  budgets (model-visible countdowns) exist in beta for pacing agent loops.
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
- **Context rot is measured**: performance degrades with input length on every
  model tested, well before window limits — focused ~300-token prompts beat 113k
  dumps. Budget effective context to a fraction of advertised; retrieve-then-focus;
  compaction and memory files are now first-class API features, not DIY.
- **Judge panels demoted — with scope**: correlated errors mean one strong
  calibrated judge at temp 0 with position-swapping beats a NAIVE majority panel
  of similar judges; heterogeneous aspect-verifier ensembles remain a live,
  distinct pattern.
- **Grounding moved into the API layer, and abstention got theory.** Provider
  citation features (Anthropic Citations, Gemini grounding, OpenAI annotations)
  now beat prompt-side quoting where available — but on Anthropic, citations and
  structured outputs are mutually exclusive. Hallucination's root cause is
  formalized (0/1 eval grading makes guessing dominate "I don't know"); sentinels
  need counter-weighting (abstention inflation), and reasoning fine-tuning
  *degrades* abstention ~24% — raising effort can worsen
  hallucination-by-non-abstention. See the grounding cards.
- **The harness became a prompt surface with its own release notes.** Commands
  merged into skills (invocation control is frontmatter now, not artifact
  choice); MCP tool search defers schemas by default (the server `instructions`
  field is the discovery surface, 2KB cap); hooks grew LLM handler types and
  deterministic context injection; subagents inherit memory files and all tools
  by default. See the harness-artifacts cards.

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
- max/xhigh effort on routine tasks — measured cost with no quality gain, sometimes
  a quality loss (overthinking).

## Skills and agent artifacts

Authoring guidance for SKILL.md files, CLAUDE.md, commands, hooks, MCP servers,
and agent definitions lives in the technique cards under the `skill-authoring` and
`harness-artifacts` levers (see `techniques/README.md`). The rules that outrank
the rest: the listing entry decides whether a skill ever loads (what + when,
third person, key use case first — it competes inside a budgeted, truncated
listing); invocation control is frontmatter, not artifact choice (commands merged
into skills); and evals come before documentation — build the three failure
scenarios first, then write the minimum skill that fixes them.
