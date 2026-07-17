# Local models — runtimes, selection, and prompting

Local models are first-class targets: `provider: local` cards point at an
OpenAI-compatible endpoint on your machine. Cost is $0 in the ledger (electricity
not modeled); privacy is total (nothing leaves the box); capability lags the
frontier by design — which is exactly why the receipt/bake-off machinery matters
most here: measure whether the local model is good enough for *this* task instead
of guessing.

## Runtimes (all speak OpenAI-compatible HTTP)

| Runtime | Default base_url | Notes |
|---|---|---|
| **Ollama** | `http://localhost:11434/v1` | easiest; pulls from ollama.com/library; `ollama list` shows installed tags |
| **llama.cpp** (llama-server) | `http://localhost:8080/v1` | maximum control; GGUF from Hugging Face; **chat template correctness is on you** — a wrong template silently wrecks instruction-following |
| **LM Studio** | `http://localhost:1234/v1` | GUI; good for browsing/quant comparison |
| **vLLM** | `http://localhost:8000/v1` | serious throughput; the choice when local means "our GPU server" not "my laptop" |

## Canonical sources for what exists locally

- **ollama.com/library** — curated, tagged, quantized builds (the practical index)
- **Hugging Face** — everything, including GGUF quants (the exhaustive index)
These join OpenRouter+Bedrock as watched sources; local availability of open-weight
models usually trails their hosted release by days.

## Selection is hardware-bound (rules of thumb)

- VRAM ≈ model GB at chosen quant + KV cache. Q4 quant ≈ params × 0.6 GB.
  8 GB → ~7-9B Q4 · 16 GB → ~14B Q4 · 24 GB → ~32B Q4 · 48+ GB → 70B Q4.
- Quantization: Q4_K_M is the sane default; below Q4, instruction-following decays
  noticeably — prefer a smaller model at Q4 over a bigger one at Q2.
- Advertised context ≠ usable context: KV cache eats VRAM fast; long-context local
  runs often force smaller quants or context caps.
- MoE models (gpt-oss-20b) run better than dense peers per GB of active params.

## Prompting differences (see technique card: small-model-prompting)

Smaller/quantized models need what frontier models have outgrown: tighter scope,
explicit structure, examples over descriptions, simpler instructions, and MORE
in-prompt reasoning scaffolding (CoT prompting HELPS most small local models —
the reasoning-model-etiquette card inverts below ~30B unless the model is a
distilled reasoner). Verify the chat template before judging a local model's
quality — most "this model is bad" verdicts are template bugs.

Seed cards (`*-local.yaml`) mark api_model_id tags `(verify with ollama list)` —
tags drift with library updates.
