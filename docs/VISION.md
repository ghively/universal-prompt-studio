# Prompt Studio v2 — Vision & Architecture

*A ground-up rethink. The v11 single-file app is treated as a directional reference only.*

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

A **local-first prompt development environment**: editor + multi-model runner +
diagnostics + evals + a versioned prompt library, where everything — prompts, model
knowledge, run history — is plain files in a git repo.

## 3. Architecture

```
prompt-studio/
├── server/          # small TypeScript server (Hono): holds API keys, calls providers,
│                    #   reads/writes the workspace, caches runs
├── web/             # React + Vite frontend
└── workspace/       # YOUR data — a git repo of plain files
    ├── prompts/     #   one .md file per prompt (frontmatter + body)
    ├── models/      #   one .yaml card per model
    ├── knowledge/   #   practices.md — the living prompting-technique rubric
    ├── evals/       #   test cases and graders
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

Persistence: `prompts/`, `models/`, `runs/log.jsonl`. No evals, no library UI, no
diffing yet. This slice already delivers the core of goal #4 and makes the model cards
load-bearing (goal #2).

### Growth

- **v0.2** — test cases + assertions + candidate-vs-incumbent scoreboard; version
  snapshots; output diff between runs. *(delivers #3)*
- **v0.3** — library view with health badges; LLM-rubric graders; blind pairwise A/B.
  *(delivers #5)*
- **v0.4** — ablation prober; linter + critique pass. *(deepens #4 and #1)*
- **v0.5** — regression sentinel; model-card refresh assistant. *(completes #2)*
- **v0.6+** — only if earned: prompt composition/chains, logprob heatmaps, export of
  suites to promptfoo format.

## 7. Prior art worth stealing from (not competing with)

- **promptfoo** — the test-case/assertion file format; proof the eval loop is right.
- **Anthropic Console** (prompt improver, eval tool) — UX for critique-then-A/B.
- **Langfuse / Braintrust** — run-history and trace-viewing patterns.

None of them combine a personal versioned library, model-aware critique, and a
diagnostics panel in a local-first tool. That combination is this product.
