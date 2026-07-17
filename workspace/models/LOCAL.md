# Local models — runtimes, selection, and prompting (verified 2026-07-17)

Local models are first-class targets: `provider: local` cards point at an
OpenAI-compatible endpoint. Cost is $0 in the ledger; privacy is total; capability
lags the frontier — which is why the receipt/bake-off machinery matters most here.

## The tier verdicts (community consensus, mid-2026)

- **8 GB default: `qwen3.5:9b`** — #1 in every category (chat, coding, reasoning,
  vision, agentic). "The 8GB question is settled."
- **4 GB default: `qwen3.5:4b`** (quality/tools) · `phi4-mini` (speed) ·
  `gemma4:e2b` (writing).
- The split that holds at both tiers: **Qwen for reasoning/tools/code, Gemma for
  prose/writing/audio.**
- `llama3.1:8b` is still the most-pulled Ollama model (inertia) — nobody current
  recommends it; Ollama now shows deprecation warnings for Llama 3.x-era agent use.

Ranked top-3 per category per tier live in the `*-local.yaml` cards (`rankings:`
field). Re-verify quarterly — this scene churns faster than hosted.

## Runtimes (all OpenAI-compatible HTTP)

| Runtime | base_url | Notes |
|---|---|---|
| **Ollama 0.32+** | `http://localhost:11434/v1` | Vulkan on by default (AMD/Intel), MLX backend with `-mlx` tags on Apple Silicon, built-in agent mode, `--think=false` flag, deep llama.cpp integration (full GGUF ecosystem) |
| **llama.cpp** | `http://localhost:8080/v1` | max control; chat-template correctness is on you; MoE expert offload (`-ot ".ffn_.*_exps.=CPU"`) runs 30B-A3B on 8GB GPUs |
| **LM Studio** | `http://localhost:1234/v1` | GUI; supports models Ollama's library lacks (e.g. GLM-4.6V-Flash official GGUF) |
| **vLLM** | `http://localhost:8000/v1` | throughput tier for GPU servers |

Quant formats: **Q4_K_M** remains the sane default; **MXFP4** and **NVFP4** are now
both in llama.cpp — they are NOT interchangeable (different block scaling).

## Hardware math (rules of thumb)

- Q4 weights ≈ params × 0.6 GB + KV cache. 8 GB → 9B-dense-class; 4 GB → 4B-class.
- **Download size ≠ VRAM** on Gemma 4 E-variants: PLE tables offload to system RAM
  (E4B: 9.6GB download, ~5GB VRAM; E2B: 7.2GB download, ~2-3GB VRAM).
- **Hybrid-attention models change the math**: Qwen3.5's Gated DeltaNet makes KV
  dramatically cheaper — 32K ctx fits where older 9B models managed 8K.
- MoE + expert offload trades VRAM for system RAM and speed (10-25 tok/s;
  needs ≥32GB RAM for 30B-A3B).
- Below Q4, instruction-following decays: smaller model at Q4 beats bigger at Q2.

## Prompting differences (technique card: small-model-prompting)

Small/quantized models invert several frontier rules: in-prompt CoT helps
(non-reasoners), examples beat descriptions, one job per call, lower temperature
for structured work, always validate + retry JSON. **Generation-specific toggle
lore**: Qwen3 honors in-prompt `/think` `/no_think`; **Qwen3.5 removed the soft
switch** — toggle via runtime params only. R1-distills take NO system prompt (all
instructions in the user turn).

## Canonical sources & known-fake tags

Sources: **ollama.com/library** + **Hugging Face** (weights/GGUF). Circulating
SEO fabrications to exclude: "qwen3.5-coder:7b" (no such model — 404), "Llama 3.3
8B" (3.3 was 70B-only), "Mistral Small 3 7B" (Small 3 is 24B). Verify every tag
with `ollama list` / the live library page before carding.
