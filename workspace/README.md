# workspace/ — your data

Everything Prompt Studio knows and makes lives here as plain files, versioned by git.

- `artifacts/` — your prompts, skills, and agent definitions, each with tests and history
- `models/` — model cards (curated) + `.measured.json` fingerprints (written by the probe battery only)
- `knowledge/` — `practices.md` (house rubric), `techniques/` (one card per technique), `probes/` (battery)
- `runs/` — every LLM call ever (monthly JSONL), the cache, observed sessions, the spend ledger
- `journal/` — coach digests and your learning log
- `queue/` — proposals awaiting your approval (nothing auto-applies)
- `evals/` — grader anchors and human judgments
- `config.json` — caps and defaults

## Routing your agents through the gateway (optional, enables the coach)
```
export ANTHROPIC_BASE_URL=http://localhost:5680/gateway
```
Anything using an OpenAI-compatible client: point its base URL at
`http://localhost:5680/gateway/v1`. Observed traffic never leaves this machine.
