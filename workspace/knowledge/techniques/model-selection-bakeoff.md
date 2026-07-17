---
slug: model-selection-bakeoff
name: Model selection by bake-off
lever: model-fit
helps: choosing (or defending) a target model for a task
hurts: n/a
---
Public leaderboards rank models on other people's tasks, with contamination
risk and no notion of your constraints; no single benchmark suffices even in
principle. Selection is empirical and local:
1. **Suite before shortlist**: assemble 10-20 task-representative cases with
   checks *first* — a bake-off without a suite is vibes with extra steps.
2. **Shortlist by hard constraints**, from the catalog, before quality enters:
   context window, modality, structured-output support, logprobs if needed,
   price ceiling, latency, deployment (local/hosted/region).
3. **Port, then race**: apply each target's card idiom (effort, format
   mechanism, sectioning) before running. Racing an un-ported prompt biases
   the bake-off toward whichever model the prompt was written for — the most
   common way bake-offs lie.
4. **Score quality AND cost AND latency** per candidate; pick from the
   frontier (quality-per-dollar at acceptable latency), not the top row.
   A cheaper model within noise of the winner is the winner.
5. **Keep the suite; re-run on releases** — selection is a standing decision,
   not an event (see version-pinning-and-drift). New-model proposals from the
   catalog watcher are bake-off triggers, not switch triggers.
Small-N caveat: 10-20 cases separate "clearly better" from "clearly worse"
only — treat close results as ties and let cost break them.
