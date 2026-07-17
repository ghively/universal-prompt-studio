---
slug: reasoning-budget-economics
name: Reasoning-budget economics (when thinking pays)
lever: reasoning
helps: routing decisions; cost models; choosing effort/model per route
hurts: n/a — decision framework, not prompt content
---
Reasoning tokens are bought accuracy. Measured curves show the last 2-5 accuracy
points can cost 4-5x the tokens, and past the knee extra thinking is neutral or
NEGATIVE (overthinking). A 16k-token trace costs 32x a 500-token one.

**Pays for itself**: multi-step math/code/planning, tasks where an error costs a
human retry or downstream failure, agentic loops where better planning cuts turn
count (front-loaded thinking can reduce total spend).

**Doesn't pay**: lookups, classification, extraction, chat, reformatting — route
to a fast model or low/none effort. Same-accuracy models differ 3x+ in tokens;
token efficiency is a model-card fact worth tracking (see Grok 4.5 vs GLM 5.2).

**Pacing control**: for capped agentic loops, a model-visible task budget
(Anthropic output_config.task_budget, beta, min 20k) makes the model wrap up
gracefully — distinct from max_tokens, which truncates without warning.
