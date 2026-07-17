# Technique library — taxonomy and coverage

120 cards, organized by **lever** (what the technique actually pulls on), not by
acronym. Acronyms rot; levers don't. Frontmatter per card: `slug`, `name`, `lever`,
`helps`, `hurts` — the compiler receives this index; card bodies load on demand.

| Lever | Cards |
|---|---|
| **clarity** (10) | action-directives · calm-imperatives · conflict-free-instructions · distribution-escape · lean-prompting · motivation-context · positive-instruction · role-scoped-to-reader · specificity-altitude · wording-robustness |
| **structure** (9) | format-sensitivity · input-format-choice · message-role-semantics · output-contract · output-length-control · system-vs-user-placement · untrusted-content-quarantine · verbosity-calibration · xml-sectioning |
| **examples** (6) | contrastive-examples · dynamic-example-retrieval · examples-as-test-cases · few-shot-examples · many-shot-prompting · worked-examples |
| **context** (10) | cache-and-append-discipline · cache-aware-ordering · content-placement · context-budget-accounting · context-compaction · drift-reanchoring · multi-turn-context-shaping · recency-and-cutoff-management · retrieval-hygiene · session-memory |
| **grounding** (8) | abstention-calibration · claim-verification-pipeline · explicit-failure-behavior · investigate-before-answering · native-citations · numeric-grounding · quote-then-answer · temporal-grounding |
| **reasoning** (9) | decomposition · effort-calibration · overthinking-mitigation · plan-then-execute · problem-reformulation · reasoning-budget-economics · reasoning-model-etiquette · thinking-trace-hygiene · verification-first |
| **verification** (11) | ambiguity-surfacing · confidence-elicitation · deterministic-check-first · golden-set-regression · human-in-the-loop-gates · judge-design · process-vs-outcome-eval · reference-anchored-judging · self-check-pass · self-consistency-sampling · uncertainty-routing |
| **orchestration** (8) | automated-prompt-optimization · batch-processing · fallback-chains · map-reduce-processing · meta-prompting · model-routing-cascades · self-correction-chain · staged-extraction |
| **agentic** (12) | agentic-persistence · approval-gates · computer-use-prompting · error-recovery-discipline · least-privilege-tooling · legible-progress · parallel-tool-steering · scope-discipline · state-and-compaction · subagent-delegation · termination-conditions · tool-description-quality |
| **skill-authoring** (11) | claude-md-authoring · skill-anatomy · skill-arguments-and-dynamic-context · skill-degrees-of-freedom · skill-description-triggering · skill-eval-driven-development · skill-invocation-control · skill-library-curation · skill-packaging-and-distribution · skill-progressive-disclosure · skill-scripts-and-loops |
| **harness-artifacts** (9) | ci-workflow-prompts · command-authoring · hook-recipes · hook-vs-prompt · mcp-resources-and-prompts · mcp-server-design · output-style-authoring · permission-rules-authoring · subagent-definition-authoring |
| **model-fit** (7) | base-model-prompting · cross-model-portability · fine-tuned-model-prompting · model-selection-bakeoff · multilingual-prompting · small-model-prompting · version-pinning-and-drift |
| **modality** (10) | 3d-spatial-prompting · audio-music-gen-prompting · diagram-code-visuals-prompting · document-ai-prompting · image-edit-prompting · image-gen-prompting · tts-voice-design-prompting · video-gen-prompting · vision-input-prompting · voice-realtime-prompting |

Harness-specific knowledge (Claude Code, AGENTS.md tools, Cursor, frameworks,
OpenClaw, consumer chat apps, automation platforms, bare API) lives in
`../harnesses/` — one card per runtime, because the harness is part of the prompt.

## Known gaps (not yet carded — honesty over coverage theater)

- **Fine-tuning vs prompting *decision* guidance** (when to tune instead of
  prompt) — out of the studio's lane for now. Prompting a model that is
  *already* fine-tuned IS covered: fine-tuned-model-prompting.
- **Harness-specific probe batteries** (does instruction-following change *inside*
  Claude Code vs bare API?) — a product feature (P8) before it's a card.
- **Long-horizon agent evaluation** (METR time-horizon-style measurement) —
  measurement methodology, not a prompting technique; belongs beside judge-design
  if it's ever carded.
- **Browser-agent prompting** (browser-use, agent-mode browsers) — the category
  churns too fast to card honestly; computer-use-prompting covers the stable core.

Update discipline: cards change when official guidance changes (cite sources), when
the ablation prober or bake-off evidence contradicts a card on the owner's tasks, or
when the field-scan job files a proposal. Every edit is a git commit — the library's
history is its changelog.
