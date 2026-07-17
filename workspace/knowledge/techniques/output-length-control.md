---
slug: output-length-control
name: Output length control
lever: structure
helps: cost/latency-sensitive pipelines; UIs with display limits; batch jobs
hurts: nothing — but the wrong mechanism silently truncates
---
Four mechanisms, not interchangeable:
1. **max_tokens is a guillotine, not a request** — the model doesn't plan for it;
   it truncates mid-JSON (stop_reason: max_tokens = discard and retry). Set it as
   a safety ceiling ~2x expected length, never as the length control — and on
   thinking models it counts reasoning + response.
2. **Parameters**: GPT-5.x text.verbosity sets prose detail without touching
   reasoning; Claude's lever is effort (scales thinking AND output — gateways
   mapping "verbosity" onto it can silently override your effort setting).
3. **Prompt, in concrete units**: "3-5 sentences", "one bullet per finding" — the
   only mechanism the model plans around. Structural units beat word counts.
4. **Stop sequences** — right for single-item outputs, wrong when the sentinel
   could appear in content.
Debugging "too short": check stop_reason first — it's usually a max_tokens
ceiling or an over-corrected "be concise" line, not model laziness.
