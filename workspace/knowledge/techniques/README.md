# Technique library — taxonomy and coverage

123 cards, organized by **lever** (what the technique actually pulls on), not by
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
| **verification** (13) | ambiguity-surfacing · confidence-elicitation · deterministic-check-first · golden-set-regression · harness-delta-probing · human-in-the-loop-gates · judge-design · long-horizon-evaluation · process-vs-outcome-eval · reference-anchored-judging · self-check-pass · self-consistency-sampling · uncertainty-routing |
| **orchestration** (8) | automated-prompt-optimization · batch-processing · fallback-chains · map-reduce-processing · meta-prompting · model-routing-cascades · self-correction-chain · staged-extraction |
| **agentic** (12) | agentic-persistence · approval-gates · computer-use-prompting · error-recovery-discipline · least-privilege-tooling · legible-progress · parallel-tool-steering · scope-discipline · state-and-compaction · subagent-delegation · termination-conditions · tool-description-quality |
| **skill-authoring** (11) | claude-md-authoring · skill-anatomy · skill-arguments-and-dynamic-context · skill-degrees-of-freedom · skill-description-triggering · skill-eval-driven-development · skill-invocation-control · skill-library-curation · skill-packaging-and-distribution · skill-progressive-disclosure · skill-scripts-and-loops |
| **harness-artifacts** (9) | ci-workflow-prompts · command-authoring · hook-recipes · hook-vs-prompt · mcp-resources-and-prompts · mcp-server-design · output-style-authoring · permission-rules-authoring · subagent-definition-authoring |
| **model-fit** (8) | base-model-prompting · cross-model-portability · fine-tuned-model-prompting · model-selection-bakeoff · multilingual-prompting · prompt-vs-finetune-decision · small-model-prompting · version-pinning-and-drift |
| **modality** (10) | 3d-spatial-prompting · audio-music-gen-prompting · diagram-code-visuals-prompting · document-ai-prompting · image-edit-prompting · image-gen-prompting · tts-voice-design-prompting · video-gen-prompting · vision-input-prompting · voice-realtime-prompting |

Harness-specific knowledge (Claude Code, AGENTS.md tools, Cursor, frameworks,
OpenClaw, consumer chat apps, automation platforms, bare API) lives in
`../harnesses/` — one card per runtime, because the harness is part of the prompt.

## Standing verification queue (no declared gaps remain — these are flagged claims awaiting measurement)

Every previously declared gap is now carded: the prompt-vs-fine-tune decision
(prompt-vs-finetune-decision), long-horizon evaluation (long-horizon-evaluation),
harness probe methodology (harness-delta-probing — P8 implements it), and
browser agents (../harnesses/browser-agents.md). What remains is a queue of
[UNVERIFIED] flags that only probes or the next refresh can settle:

- claude-opus-4-8 temperature: card says 400, catalog says supported — one probe call settles it.
- kimi-k2.7-code sampling params on the OpenRouter route (catalog lists them; Moonshot first-party rejects them).
- text.verbosity pass-through on OpenRouter GPT-5.6 routes.
- Gemma 4 E-variant PLE fidelity in llama.cpp (A/B against -mlx tags).
- Ollama speculative-decoding support; KV-cache-quant env behavior on hybrid-attention layers.
- The five thin hosted cards (hy3, nex-n2-mini, ling-2.6-flash, gpt-5.4-nano, cohere-command-a) await probe-battery promotion.

Update discipline: cards change when official guidance changes (cite sources), when
the ablation prober or bake-off evidence contradicts a card on the owner's tasks, or
when the field-scan job files a proposal. Every edit is a git commit — the library's
history is its changelog.
