---
slug: overthinking-mitigation
name: Overthinking detection and mitigation
lever: reasoning
helps: high-effort routes; long agentic sessions; latency-sensitive products
hurts: n/a — diagnostic card
---
Reasoning models over-generate on easy inputs: redundant verification loops,
re-derived facts, re-litigated decisions. This is measured (TMLR 2025 survey and
successors), costs real money, and past a point REDUCES accuracy.

**Detect**: thinking tokens >> output tokens on simple inputs; accuracy flat or
falling as effort rises on your eval; traces re-checking verified steps; agents
re-opening settled decisions; context anxiety late in long sessions.

**Mitigate, in order**: (1) lower effort — the API knob beats any prompt; (2)
route easy inputs to a fast model or none/low; (3) scope prompts: "When you have
enough information to act, act. Do not re-derive established facts or re-litigate
decided questions."; (4) on adaptive models, steer triggering: "Thinking adds
latency — when in doubt, respond directly." Never lead with "think less" caps —
they undershoot on the hard tail.
