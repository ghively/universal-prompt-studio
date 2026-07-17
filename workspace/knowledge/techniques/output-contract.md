---
slug: output-contract
name: Output contract
lever: structure
helps: output consumed by software; any format-sensitive task
hurts: creative tasks where structure would flatten the result
---
When anything downstream depends on the output's shape, use the provider's **native
Structured Outputs feature** (Anthropic structured outputs / OpenAI json_schema
strict mode) — schema-constrained decoding, not hope. In-prompt schemas remain the
fallback: exact field names, "return only JSON", one valid instance.

**What changed in 2026**: assistant-turn prefills (the old trick for forcing formats)
are **removed** on current Claude models — requests 400. Migrate format-forcing to
structured outputs or forced tool-use with enum fields for classification.

A contract is also a free machine check — the schema doubles as the test.
