---
slug: verbosity-calibration
name: Verbosity calibration
lever: structure
helps: recurring prompts where response length matters; migrated prompts carrying "be concise"
hurts: nothing
---
Two changes since the be-concise era: (1) current models are **already concise** —
blanket "keep it short" instructions now overshoot into uselessly terse; (2) length
is becoming a **parameter**, not a prompt line — GPT-5.6's text.verbosity and the
verbosity param on current Claude via OpenRouter set the default detail level
consistently across calls.

**Practice**: set the parameter where the card says it exists; in the prompt, specify
length only in concrete units ("3–5 sentences", "one paragraph per finding") and only
where it genuinely matters. If tool-use summaries are being skipped, ask for them
explicitly rather than raising global verbosity.
