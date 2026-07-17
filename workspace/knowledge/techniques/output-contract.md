---
slug: output-contract
name: Output contract
lever: structure
helps: output consumed by software; any format-sensitive task
hurts: creative tasks where structure would flatten the result
---
When anything downstream depends on the output's shape, use the provider's **native
Structured Outputs feature** (Anthropic `output_config.format` json_schema, or
`strict: true` tool use; OpenAI json_schema strict mode). Guarantees are
**conditional**: refusals, max_tokens truncation, and case-differing enums still
produce non-conforming output — and the schema is only a *syntax* test; field
correctness still needs a check. In-prompt schemas remain the fallback.

**What changed in 2026**: assistant-turn prefills (the old trick for forcing formats)
are **removed** on current Claude models — requests 400. Migrate format-forcing to
structured outputs or forced tool-use with enum fields for classification.

A contract is also a free machine check — the schema doubles as the test.

**Schemas are prompts**: key wording measurably changes accuracy at fixed
prompt/model; field descriptions and enum choices are an instruction channel.
Beware required-properties-first reordering (Anthropic) silently defeating
"reasoning field before answer field" — and pair structured output with thinking
so the constraint binds the wire format, never the reasoning.
