---
slug: automated-prompt-optimization
name: Automated prompt optimization
lever: orchestration
helps: multi-stage pipelines with objective metrics; model-migration (re-optimize for a new/cheaper model); squeezing small models
hurts: tasks without a trustworthy metric (you'll optimize into judge biases); tiny/subjective tasks; teams that must hand-maintain the prompt
---
Current state (mid-2026): **GEPA** (reflective prompt evolution, ICLR 2026 oral)
superseded MIPROv2 as DSPy's default recommendation — beats MIPROv2 by >10%, and beats GRPO-style RL by 6-20% with up to 35x fewer
rollouts; works best when your **metric returns explanatory text**, not
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

**Economics**: rollout budget is dominated by eval calls — optimizer runs are
canonical batch-API workloads (50% off). For a single simple prompt, a
SIMBA-class cheap reflective pass before a full GEPA run is the current
community recommendation; GEPA's strongest case is multi-predictor programs.
