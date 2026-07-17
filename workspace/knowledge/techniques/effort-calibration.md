---
slug: effort-calibration
name: Effort calibration (find the level empirically)
lever: reasoning
helps: any route on an effort-capable model; cost- or latency-sensitive products
hurts: n/a — this card is method, not prompt content
---
Effort is the primary reasoning control and it is NOT monotonic: higher effort can
*lower* total cost on agentic work (smarter planning → fewer turns), while max
often shows diminishing returns and overthinking. Never pick a level by vibes.

**Method**: sweep per route, not per app. Run your eval set at medium, high, and
xhigh (add low for subagents/simple routes; max only for correctness-over-cost).
Record quality, latency, and TOTAL tokens including tool turns. Pick per-route
winners; re-sweep on every model migration — effort semantics shift between
generations.

**Anchors (Anthropic current gen)**: default is high when unset; xhigh is the
documented coding/agentic sweet spot; low still beats prior generations' top
settings on routine work. Symptoms: shallow answers at low/medium on complex
tasks → raise effort (don't prompt around it); unrequested refactors at
xhigh/max → lower effort first, then add scope constraints.
