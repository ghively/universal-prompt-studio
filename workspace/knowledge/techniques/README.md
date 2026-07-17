# Technique library — taxonomy and coverage

58 cards, organized by **lever** (what the technique actually pulls on), not by
acronym. Acronyms rot; levers don't. Frontmatter per card: `slug`, `name`, `lever`,
`helps`, `hurts` — the compiler receives this index; card bodies load on demand.

| Lever | Cards |
|---|---|
| **clarity** | role-scoped-to-reader · positive-instruction · motivation-context · lean-prompting · calm-imperatives · action-directives · distribution-escape |
| **structure** | output-contract · xml-sectioning · system-vs-user-placement · untrusted-content-quarantine · verbosity-calibration |
| **examples** | few-shot-examples · contrastive-examples |
| **context** | content-placement · cache-aware-ordering · retrieval-hygiene · drift-reanchoring · context-compaction · cache-and-append-discipline |
| **grounding** | quote-then-answer · explicit-failure-behavior · investigate-before-answering |
| **reasoning** | reasoning-model-etiquette · decomposition · plan-then-execute |
| **verification** | self-check-pass · ambiguity-surfacing · self-consistency-sampling · judge-design · confidence-elicitation |
| **orchestration** | self-correction-chain · meta-prompting · automated-prompt-optimization |
| **agentic** | agentic-persistence · state-and-compaction · parallel-tool-steering · scope-discipline · tool-description-quality · subagent-delegation |
| **skill-authoring** | skill-anatomy · skill-description-triggering · skill-progressive-disclosure · skill-degrees-of-freedom · skill-scripts-and-loops · skill-eval-driven-development · claude-md-authoring |
| **harness-artifacts** | command-authoring · hook-vs-prompt · mcp-server-design · subagent-definition-authoring |
| **model-fit** | small-model-prompting · multilingual-prompting |
| **modality** | image-gen-prompting · vision-input-prompting · audio-music-gen-prompting · video-gen-prompting · voice-realtime-prompting |

Harness-specific knowledge (Claude Code, AGENTS.md tools, Cursor, frameworks,
bare API) lives in `../harnesses/` — one card per runtime, because the harness is
part of the prompt.

## Known gaps (not yet carded — honesty over coverage theater)

- **Fine-tuning vs prompting *decision* guidance** (when to tune instead of
  prompt) — out of the studio's lane for now. Prompting a model that is
  *already* fine-tuned IS covered: fine-tuned-model-prompting.
- **Harness-specific probe batteries** (does instruction-following change *inside*
  Claude Code vs bare API?) — a product feature (P8) before it's a card.
- **3D/CAD and world-model generation prompting** — no owner need yet.

Update discipline: cards change when official guidance changes (cite sources), when
the ablation prober or bake-off evidence contradicts a card on the owner's tasks, or
when the field-scan job files a proposal. Every edit is a git commit — the library's
history is its changelog.
