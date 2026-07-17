---
slug: automated-prompt-optimization
name: Automated prompt optimization
lever: orchestration
helps: multi-stage pipelines with objective metrics; model-migration (re-optimize for a new/cheaper model); squeezing small models
hurts: tasks without a trustworthy metric (you'll optimize into judge biases); tiny/subjective tasks; teams that must hand-maintain the prompt
---
Current state (mid-2026): **GEPA** (reflective prompt evolution, ICLR 2026 oral)
superseded MIPROv2 as DSPy's default recommendation — beats it by ~13% with ~35x
fewer rollouts, and works best when your **metric returns explanatory text**, not
just a scalar (reflective optimizers feed on feedback). DSPy 3.2.x also ships
SIMBA and BetterTogether chaining; TextGrad/AdalFlow/promptim are the
single-optimizer alternatives.

Requirements before touching any of it: a programmatic metric, 50–300 labeled
examples, a held-out test set (optimized prompts overfit validation like weights
do), and version control — the optimized prompt is a **build artifact**: often
unreadable, model-specific, and non-transferable across providers. Re-run on
every model swap; that migration case is where it most clearly beats manual
iteration. This studio's manual loop (compile → receipt → bake-off) is the
right default until a prompt has a real suite; then optimization is a Lab-tier
power tool.
