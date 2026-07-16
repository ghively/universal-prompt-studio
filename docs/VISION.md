# Prompt Studio v2 — Vision & Architecture

*A ground-up rethink. The v11 single-file app is treated as a directional reference only.*

*Rev 2: adds the technique catalog + task profiler/generator (§4.6), the learning layer
(§4.7), and skills/agent-artifact evaluation (§4.8).*

---

## 1. The reframe

The current app is a **prompt assembler**: it helps you construct a prompt-shaped artifact,
and then its job is done. It never talks to a model, so it can never learn whether the
prompt works. All five of your goals require the opposite loop:

> **write → run → observe → measure → revise**

So the core object of the new tool is not a form. It is a **prompt under test** —
versioned, parameterized with variables, carrying its own test cases and checks, with a
run history attached. Everything else in this document falls out of that one decision.

A second, blunter correction: the 450-field schema wall encodes a 2022–23 view of
prompting — keyword stuffing, `"masterpiece, best quality"`, magic parameters. Modern
models reward clear natural-language instructions, good examples, sensible structure, and
iteration against real outputs. The wall of levers isn't just overwhelming; for current
models it produces *worse* prompts. It goes.

**Verdict on the old code: full rewrite.** Keep two *ideas* (the `[Ask me about this]`
sentinel as a UX pattern; task-type scaffolds as a slimmed-down descendant of the
schemas). Keep none of the code.

## 2. The product in one sentence

A **local-first prompt development environment that teaches you the craft as you use
it**: editor + multi-model runner + diagnostics + evals + a versioned library of
instruction artifacts (prompts, system prompts, skills, agent definitions), where
everything — artifacts, model knowledge, technique knowledge, run history — is plain
files in a git repo.

## 3. Architecture

```
prompt-studio/
├── server/          # small TypeScript server (Hono): holds API keys, calls providers,
│                    #   reads/writes the workspace, caches runs
├── web/             # React + Vite frontend
└── workspace/       # YOUR data — a git repo of plain files
    ├── prompts/     #   one .md file per prompt (frontmatter + body)
    ├── skills/      #   agent skills / system prompts / agent defs under test (§4.8)
    ├── models/      #   one .yaml card per model
    ├── knowledge/
    │   ├── practices.md    # the living prompting/context-engineering rubric
    │   └── techniques/     # one card per technique (§4.6) — data, not code
    ├── evals/       #   test cases, graders, and skill scenarios
    ├── journal/     #   your annotated learning log (§4.7)
    └── runs/        #   append-only JSONL: every execution ever
```

**Why abandon the single HTML file:** API keys must not live in the browser; provider
calls need a server (CORS, streaming, retries); the library needs file persistence; evals
need background runs. The no-build constraint was right for an offline form-filler and is
wrong for this.

**Why local-first, not SaaS:** it's your library and your keys; git gives you versioning,
diffing, backup, and portability for free; and every hosted prompt-ops tool (Langfuse,
PromptLayer, Braintrust) is organized around team telemetry, not a personal craft loop.

**Provider layer:** one adapter interface. Anthropic + OpenAI + Google direct, plus
**OpenRouter** as the long-tail path (one key ≈ every other model). Start with the Vercel
AI SDK as the unified layer; drop to raw SDKs wherever it hides fields the diagnostics
need (thinking blocks, logprobs).

**Stack:** TypeScript end-to-end, Hono server, React+Vite UI. Boring on purpose.

## 4. The five goals, translated into mechanisms

### 4.1 "Bring it current" → a linter + critique pass, kept as *data*

Best practices rot, so they don't get hardcoded. They live in
`knowledge/practices.md` — versioned, editable — and two mechanisms apply them:

- **Static linter** (instant, free): flags negation-only instructions ("don't X" without
  a positive alternative), missing examples on format-sensitive tasks, instructions
  buried *before* long context instead of after it, cache-hostile ordering (variable
  content before the stable prefix), "think step by step" aimed at a reasoning model
  (harmful — set a thinking budget instead), "reply in JSON" begging when the target
  supports structured outputs.
- **Critique pass** (one cheap model call): a strong model reviews your prompt against
  the rubric *and the target model's card* and proposes concrete edits — like
  Anthropic's prompt improver, but yours, and model-aware. Crucially, every suggested
  rewrite lands as a *candidate version* you A/B against the incumbent (§4.3), never a
  silent replacement.

Ten-ish slim **scaffolds** per task type (extraction, classification, long-doc Q&A,
agent/tool-use, image gen, code gen…) replace the nine mega-schemas: each is a page of
structure with holes, not 80 dropdowns.

### 4.2 "Know the models" → model cards as small, opinionated YAML

One `models/<id>.yaml` per model:

```yaml
id: claude-sonnet-5
provider: anthropic
context_window: 200000
max_output: 64000
pricing: { input_per_mtok: 3.00, output_per_mtok: 15.00 }
sampling:
  defaults: { temperature: 1.0 }
  notes: "temperature and top_p are mutually exclusive; thinking mode forces temp=1"
reasoning: { kind: hybrid-thinking, knob: thinking_budget_tokens, cot_prompting: harmful }
formatting_idiom: "XML tags for sectioning; system prompt for role; prefill to steer format"
structured_output: tool-use schema (strict)
strengths: [long-context synthesis, instruction fidelity, code]
failure_modes: [over-hedging without explicit confidence instruction, ...]
prompting_notes: |
  - Put documents first, instructions last, ask it to quote before answering.
  - ...
last_reviewed: 2026-07-16
sources: [https://docs.anthropic.com/...]
```

Rules that keep the library alive instead of rotting:

- **Small and opinionated** — a page, not an encyclopedia. Cross-model truths live in
  `practices.md`, never duplicated per card.
- **Staleness is surfaced**: the UI badges every card with its `last_reviewed` age.
- **Refresh assistant**: when a model ships, an LLM drafts the card from release notes
  and docs; you review and approve the diff. Adding a model is editing one file.
- Cards are *load-bearing*: the runner prefills sampling params from the card, the
  linter reads its `reasoning`/`formatting_idiom` fields, the critique pass cites it.

### 4.3 "Robust, not fragile" → measurement is ambient, comparison is the default

The knob-fiddling failure mode ("did I just make it worse?") is fixed by making
*every* change comparable:

- A prompt can carry **test cases** (sets of variable values) and **checks**: cheap
  assertions (contains / regex / JSON-valid / schema-conforms / length), LLM-rubric
  graders, and **pairwise judging** (blind, position-swapped) against the previous
  version.
- Editing a prompt produces a **candidate vs. incumbent** view: run the suite, get a
  scoreboard, promote or discard. Tuning on vibes becomes structurally difficult.
- Runs are **cached** by `(prompt-hash, model, params, input)` so re-running a suite
  after a one-line edit only pays for what changed. A **cost meter** is always visible.

### 4.4 "Inside the model's reaction" → an honest instruments panel

This is the differentiator, and honesty about what's real matters. APIs do not expose
attention or activations; anything claiming otherwise is dressed-up self-report. What
*is* available, in descending order of trustworthiness:

1. **Ablation prober** (real evidence, the best instrument): auto-generate prompt
   variants — drop each section, reorder, paraphrase the key instruction — run each
   against the test cases, and show which parts of your prompt are **load-bearing vs.
   dead weight**. This answers "what is my prompt actually doing" empirically.
2. **Consistency probe** (real, works on any model): N samples at temperature > 0,
   semantic-cluster the answers → an agreement score and a view of *where* the model
   diverges. A practical uncertainty signal.
3. **Reasoning traces** (real, direct): extended thinking (Claude), R1/Gemini thinking,
   o-series summaries — rendered beside the output, with the phrases from *your prompt*
   that the trace references highlighted.
4. **Logprobs** (real, OpenAI-compatible providers only): token-confidence heatmap and
   top-k alternatives at pivotal tokens — literally "what almost changed its answer."
   Anthropic doesn't expose logprobs; the UI says so instead of faking it.
5. **Interpretation report** (self-report — labeled as such): a structured side-call:
   *"Before answering: restate the task as you understood it; list ambiguities; list
   assumptions you made; list instructions that conflicted or that you ignored."*
   Not ground truth, but empirically the fastest way to discover your prompt is being
   misread.
6. **Cross-model diff**: the same prompt on 2–3 models with interpretations diffed —
   a misreading shared by all models is your prompt's fault, not the model's.

### 4.5 "Living library" → prompts as versioned, *tested* files

- `prompts/<name>.md`: frontmatter (title, tags, variables, target models, checks,
  changelog) + body. Git is the version store; the UI shows a version timeline with
  eval scores per version.
- **Health badges** per prompt: last run date, pass rate, models verified against.
- **Regression sentinel**: when a new model card lands, one click re-runs the library's
  suites on it → *"7 prompts hold up, 2 regressed (diffs attached), 1 improved."*
  This is the mechanism behind "which ones still hold up."

### 4.6 Technique generator: catalog + task profiler + bake-off

The request behind this feature is "a generator for different techniques and a way to
determine which is the best method for what I'm trying to accomplish." One honest
correction first: **there is no oracle for 'best technique.'** Anyone who tells you
CoT-vs-few-shot-vs-decomposition can be decided from a lookup table is selling vibes in
a different costume. What *can* be built is a recommender that proposes strong
candidates and an arena that decides empirically — and a memory that makes the second
decision cheaper than the first.

**a) Technique catalog — `knowledge/techniques/*.md`, one card per technique.**
Organized by *lever*, not by acronym zoo (technique names rot; levers don't):

- **Clarity & structure** — role framing, positive instruction, sectioning, output
  contracts
- **Examples** — few-shot selection, contrastive examples, example ordering
- **Reasoning** — CoT (and when it's harmful), decomposition, plan-then-execute,
  thinking budgets on reasoning models
- **Context engineering** — what goes in the window and where: doc placement,
  quote-then-answer, retrieval snippet hygiene, cache-aware layout, token budgeting,
  compaction, tool-definition design
- **Verification** — self-critique passes, rubric-in-prompt, structured outputs as
  guardrails
- **Sampling & orchestration** — temperature strategy, best-of-N, multi-call patterns

Each card: what it is, the mechanism (why it works), **when it helps / when it hurts /
when it's a no-op**, cost profile, a worked before→after example, and sources. Cards
are data — updated as the field moves, same as model cards.

**b) Task profiler — the front door for a new prompt.**
You describe the goal in plain language. The profiler asks 2–4 clarifying questions
(the old app's `[Ask me about this]` sentinel, reborn as actual conversation), then
classifies the task along the dimensions that actually drive technique choice:
output verifiability (checkable vs. subjective), reasoning depth, format sensitivity,
context volume, latency/cost budget, one-shot vs. reusable, agentic vs. single-call.

**c) Candidate generation + bake-off — the recommender proposes, the eval loop
decides.** From the profile, it drafts **2–3 candidate prompts using different
technique combinations** (e.g. minimal-instruction vs. few-shot-heavy vs.
decompose-and-verify), each annotated with *which cards it used and why*. You run them
head-to-head through §4.3's scoreboard. No "trust me, this is best" — a result you can
see.

**d) The evidence base — the part that compounds.** Every bake-off verdict is recorded
against the task profile: over time the recommender ranks candidates using *your own
history* ("on your extraction tasks, few-shot beat CoT 8 of 9 times"), not just the
catalog's priors. This is the difference between a tips website and a tool that learns
your workload.

### 4.7 Learning layer — the tool as curriculum

You're learning prompt/context engineering from scratch; the tool should teach the
craft *through* use, not beside it. Mechanisms, cheapest first:

- **Explain-everything.** Every lint warning, critique suggestion, and technique
  recommendation links to its technique card — the *why*, not just the *what*. Terms
  get hover-glossary treatment. Zero extra API cost; pure UI discipline.
- **The instruments are the teachers.** The ablation prober (§4.4) shows which parts of
  a prompt are load-bearing; the consistency probe shows what "model uncertainty"
  physically looks like; the cross-model diff shows idiom differences. Watching these
  on your own prompts beats any tutorial.
- **Predict-then-reveal drills.** Because the tool can generate variants and measure,
  it can run calibration exercises: here's a task and two prompts — predict which wins
  and why, then run it and see. Being wrong in a measurable way is the fastest teacher
  there is. Drills are generated from the technique catalog, so new cards become new
  drills automatically.
- **Curriculum path.** An ordered walk through the catalog (fundamentals → structure →
  examples → reasoning → context engineering → evals → agents/skills), where each stop
  is a drill plus a "now apply it to one of *your* prompts" step.
- **Journal.** `journal/` holds annotated entries the tool half-writes for you: what
  you changed, what the scoreboard said, what you concluded. Six months in, this is
  your personal textbook — searchable, and mined by the recommender (§4.6d).

### 4.8 Beyond prompts: skills, system prompts, and agent artifacts

A prompt is just one kind of **instruction artifact**. The same
version → run → measure → regress loop applies to:

- **Agent skills** (SKILL.md + resources, à la Claude Code / claude.ai skills)
- **System prompts / CLAUDE.md files / agent definitions**
- **Tool descriptions** (the most under-evaluated prompt surface in agentic systems)

Skills need a different harness than single-call prompts, because a skill is exercised
across a multi-turn, tool-using trajectory:

- **Scenario-based evals**: each skill carries scenarios — tasks that *should* trigger
  it, tasks that *shouldn't* (trigger precision matters as much as recall), and
  expected-outcome checks.
- **Three measurements per scenario**: (1) *activation* — did the agent load the skill
  when it should and only then; (2) *compliance* — did the trajectory actually follow
  the skill's instructions (LLM-rubric over the transcript); (3) *outcome* — did the
  task end in the right state (assertions on files/outputs).
- **Runner**: headless agent runs via the Claude Agent SDK (and equivalents later),
  transcripts captured into `runs/` like any other execution.
- Everything else is inherited free: skills are already files in git, so versioning,
  health badges, the regression sentinel ("re-test my skills against the new model"),
  and A/B of two skill phrasings all come from the machinery in §4.3–4.5.

This is the least-served niche in the ecosystem — prompt evals have promptfoo et al.;
**nobody has a good personal skill-eval harness.** It is also the heaviest lift
(agentic trajectories cost more to run and grade), which is why it sits late in the
build path — but the workspace model is designed for it from day one.

## 5. Cut list (from the old app)

- The nine mega-schemas and all 450 fields; the 37-preset grid. Replaced by scaffolds.
- The chain builder with 23 "translate to Canva/Figma/n8n" targets — a gimmick. Real
  chaining (prompt composition with data flow) can earn its way in at v0.6+ if needed.
- The image/video tag machinery (`medium_aesthetics`, quality keywords). Modern image
  models want natural-language descriptions; image prompting becomes one scaffold plus
  model cards for Imagen/FLUX/etc.
- The no-build, single-file constraint.

## 6. Build path

### v0.1 — the smallest slice you can feel (a weekend)

One screen, three panes:

1. **Editor**: markdown prompt with `{{variables}}`.
2. **Run controls**: model picker fed by ~5 hand-written seed cards (Claude Sonnet,
   GPT, Gemini, DeepSeek-R1 via OpenRouter, one small/cheap model); params prefilled
   from the card.
3. **Output + Reaction panel**: the response, plus — thinking trace (when the model
   has one), the interpretation report, and an optional 3-sample consistency score.

Persistence: `prompts/`, `models/`, `runs/log.jsonl` — plus the first ~15 technique
cards as static content (they're just markdown; writing them costs nothing and the
learning layer starts working on day one via explain-everything links). No evals, no
library UI, no diffing yet. This slice already delivers the core of goal #4, makes the
model cards load-bearing (goal #2), and begins the curriculum (§4.7).

### Growth

- **v0.2** — test cases + assertions + candidate-vs-incumbent scoreboard; version
  snapshots; output diff between runs. *(delivers §4.3 — everything after this can
  measure itself)*
- **v0.3** — task profiler + candidate generation + bake-off, built on v0.2's
  scoreboard; library view with health badges; LLM-rubric graders; blind pairwise A/B.
  *(delivers §4.6 and §4.5 — the generator arrives only once it can be judged)*
- **v0.4** — ablation prober; linter + critique pass; predict-then-reveal drills and
  the journal. *(deepens §4.4, delivers §4.1 and the active half of §4.7)*
- **v0.5** — regression sentinel; model-card refresh assistant; bake-off evidence base
  feeding the recommender. *(completes §4.2, compounds §4.6d)*
- **v0.6** — skill/agent harness: scenario evals, activation/compliance/outcome
  measurement, headless Agent SDK runner. *(delivers §4.8; pull it forward if skill
  evaluation becomes the priority — it depends only on v0.2's checks machinery)*
- **v0.7+** — only if earned: prompt composition/chains, logprob heatmaps, export of
  suites to promptfoo format.

## 7. Prior art worth stealing from (not competing with)

- **promptfoo** — the test-case/assertion file format; proof the eval loop is right.
- **Anthropic Console** (prompt improver, eval tool) — UX for critique-then-A/B.
- **Langfuse / Braintrust** — run-history and trace-viewing patterns.

None of them combine a personal versioned library, model-aware critique, a diagnostics
panel, a technique recommender that learns from your own bake-offs, and a skill-eval
harness in a local-first tool. That combination is this product.
