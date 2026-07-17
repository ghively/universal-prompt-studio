# Technique library — taxonomy and coverage

51 cards, organized by **lever** (what the technique actually pulls on), not by
acronym. Acronyms rot; levers don't. Frontmatter per card: `slug`, `name`, `lever`,
`helps`, `hurts` — the compiler receives this index; card bodies load on demand.

| Lever | Cards |
|---|---|
| **clarity** | role-scoped-to-reader · positive-instruction · motivation-context · lean-prompting · calm-imperatives · action-directives · distribution-escape |
| **structure** | output-contract · xml-sectioning · system-vs-user-placement · untrusted-content-quarantine · verbosity-calibration |
| **examples** | few-shot-examples · contrastive-examples |
| **context** | content-placement · cache-aware-ordering · retrieval-hygiene · drift-reanchoring |
| **grounding** | quote-then-answer · explicit-failure-behavior · investigate-before-answering |
| **reasoning** | reasoning-model-etiquette · decomposition · plan-then-execute |
| **verification** | self-check-pass · ambiguity-surfacing · self-consistency-sampling · judge-design · confidence-elicitation |
| **orchestration** | self-correction-chain · meta-prompting |
| **agentic** | agentic-persistence · state-and-compaction · parallel-tool-steering · scope-discipline · tool-description-quality · subagent-delegation |
| **skill-authoring** | skill-anatomy · skill-description-triggering · skill-progressive-disclosure · skill-degrees-of-freedom · skill-scripts-and-loops · skill-eval-driven-development · claude-md-authoring |
| **harness-artifacts** | command-authoring · hook-vs-prompt · mcp-server-design · subagent-definition-authoring |
| **model-fit** | small-model-prompting |
| **modality** | image-gen-prompting · vision-input-prompting |

Harness-specific knowledge (Claude Code, AGENTS.md tools, Cursor, frameworks,
bare API) lives in `../harnesses/` — one card per runtime, because the harness is
part of the prompt.

## Known gaps (not yet carded — honesty over coverage theater)

- **Audio/music and video *generation* prompting** — image-gen and vision-input are
  carded; audio/video generation waits for owner need.
- **Automated prompt optimization internals** (DSPy/GEPA-style search loops) —
  meta-prompting covers the manual form; the automated loop is a product feature
  before it's a technique card.
- **Fine-tuning vs prompting decision guidance** — out of the studio's lane for now.
- **Multilingual prompting** — no owner need yet.
- **Voice/realtime agent prompting** (interruption handling, latency budgets) — no
  owner need yet.

Update discipline: cards change when official guidance changes (cite sources), when
the ablation prober or bake-off evidence contradicts a card on the owner's tasks, or
when the field-scan job files a proposal. Every edit is a git commit — the library's
history is its changelog.
