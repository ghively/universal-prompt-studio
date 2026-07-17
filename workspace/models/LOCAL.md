# Local models — runtimes, selection, and prompting (verified 2026-07-17)

Local models are first-class targets: `provider: local` cards point at an
OpenAI-compatible endpoint. Cost is $0 in the ledger; privacy is total; capability
lags the frontier — which is why the receipt/bake-off machinery matters most here.

## The tier verdicts (community consensus, mid-2026)

- **8 GB default: `qwen3.5:9b`** — #1 in every category (chat, coding, reasoning,
  vision, agentic). "The 8GB question is settled."
- **4 GB default: `qwen3.5:4b`** (quality/tools) · `phi4-mini` (speed) ·
  `gemma4:e2b` (writing).
- **16 GB default: `gpt-oss:20b`** (14GB MXFP4, fastest in tier, reasoning-effort
  levels) · `gemma4:26b` (the multimodal generalist, light offload) ·
  `qwen3-coder:30b` near-resident for coding · `qwen3-vl:8b` / `glm-4.6v-flash`
  for vision.
- **24 GB default: `qwen3.6:27b`** (general, coding, reasoning) · `qwen3-vl:30b`
  (vision) · `qwen3-coder:30b` fully VRAM-resident. CAUTION: `qwen3.5:35b` and
  `qwen3.6:35b` are 24GB *downloads* — they do NOT fit a 24GB card with KV cache;
  they're 32GB-Mac / 48GB-GPU models.
- **12 GB is a delta on 8 GB**, not its own set: `gemma4:12b` stops being tight,
  `deepseek-r1:14b` (9.0GB) displaces `:8b` for reasoning, `qwen2.5-coder:14b`
  (9.0GB) for FIM; everything else inherits from 8GB with longer context.
- **48 GB+ splits in two**: 48GB GPU → `deepseek-r1:70b` (43GB), `qwen3.6:35b` at
  Q8; 64GB+ (Mac/RAM) → `qwen3-coder-next` (52GB q4_K_M, the agentic-coding pick),
  `gpt-oss:120b` (~65GB). Don't treat them as one tier — the 52GB download sits
  exactly on the fault line.
- The split that holds across tiers: **Qwen for reasoning/tools/code, Gemma for
  prose/writing/audio.**
- `llama3.1:8b` remains among the most-pulled Ollama models (inertia — current
  popular sort leads with Gemma 4 and Qwen3.5) — nobody current recommends it;
  Ollama now shows deprecation warnings for Llama 3.x-era agent use.
- **Qwen3.6** (Feb→June 2026 line; 27b/35b only, no small sizes) does not disturb
  the 8GB/4GB verdicts — but from 24GB up it, not Qwen3.5, is the community
  default.

Ranked top-3 per category per tier live in the `*-local.yaml` cards (`rankings:`
field). Re-verify quarterly — this scene churns faster than hosted.

## Apple Silicon is its own hardware class, not a VRAM tier

Unified memory shares one pool — budget ~2/3-3/4 of RAM for the model. Ollama's
Apple backend is MLX (v0.19+, ~2x decode vs llama.cpp on M-series), and the
library's `-mlx` tags are **distinct artifacts with different sizes** (cards carry
an optional `mlx_tag:` field). Mapping: 16GB Mac ≈ the 8GB verdicts, 32GB ≈ the
24GB verdicts (30B-A3B class at ~100 tok/s), 64GB ≈ 48GB+ (70B Q4). On Apple
Silicon prefer `-mlx` tags outright — see also the Gemma 4 PLE fidelity caveat
below.

## Runtimes (all OpenAI-compatible HTTP)

| Runtime | base_url | Notes |
|---|---|---|
| **Ollama 0.32+** | `http://localhost:11434/v1` | Vulkan on by default (AMD/Intel), MLX backend with `-mlx` tags on Apple Silicon, built-in agent mode, `--think=false` flag, deep llama.cpp integration (full GGUF ecosystem) |
| **llama.cpp** | `http://localhost:8080/v1` | max control; chat-template correctness is on you; MoE expert offload (`-ot ".ffn_.*_exps.=CPU"`) runs 30B-A3B on 8GB GPUs |
| **LM Studio** | `http://localhost:1234/v1` | GUI; supports models Ollama's library lacks (e.g. GLM-4.6V-Flash community GGUF) |
| **vLLM** | `http://localhost:8000/v1` | throughput tier for GPU servers |

Quant formats: **Q4_K_M** remains the sane default; **MXFP4** and **NVFP4** are now
both in llama.cpp — they are NOT interchangeable (different block scaling).

## Runtime traps and free wins (power-user layer)

- **The #1 practical trap: Ollama's default context length is far below every
  number on this page.** Set `num_ctx` (or `OLLAMA_CONTEXT_LENGTH`) explicitly per
  request, or your 256K model silently truncates the prompt at a few thousand
  tokens with no error.
- **KV-cache quantization is the cheapest context win**: `OLLAMA_FLASH_ATTENTION=1`
  + `OLLAMA_KV_CACHE_TYPE=q8_0` (llama.cpp: `-fa --cache-type-k q8_0
  --cache-type-v q8_0`) roughly halves KV memory with negligible quality loss;
  q4_0 KV visibly hurts long-context recall. Hybrid-attention models (Qwen3.5's
  Gated DeltaNet) already shrink KV — stack the savings before assuming you need
  offload. (Verify env-var behavior on hybrid-attention layers on refresh.)
- **Speculative decoding is free single-user throughput**: llama.cpp
  `--model-draft` with a same-family draft (qwen3.5:0.8b drafting for 9b/27b) and
  LM Studio's toggle give 1.5-2.5x decode on greedy-ish workloads. (Whether Ollama
  has shipped it: unverified — check on refresh.)
- **Multi-GPU**: llama.cpp splits with `--split-mode row --tensor-split`, vLLM
  with `--tensor-parallel-size`; Ollama auto-spreads layers but is
  PCIe-bandwidth-bound — two 12GB cards are not one 24GB card for dense models.
- **Context extension beyond native** (Qwen's 256K→1M via YaRN) trades recall
  quality for length — set the scaling factor to the minimum your longest real
  prompt needs, never the maximum advertised.
- **Gemma 4 E-variant GGUF fidelity**: an unresolved upstream question over
  whether llama.cpp implements PLE per-layer injection (outputs may be subtly
  degraded vs vLLM/MLX/transformers). Prefer `-mlx` on Apple Silicon; re-probe
  prose quality on refresh.

## Hardware math (rules of thumb)

- Q4 weights ≈ params × 0.6 GB + KV cache. 8 GB → 9B-dense-class; 4 GB → 4B-class.
- **Download size ≠ VRAM** on Gemma 4 E-variants: PLE tables offload to system RAM
  (E4B: 9.6GB download, ~5GB VRAM; E2B: 7.2GB download, ~2-3GB VRAM).
- **Hybrid-attention models change the math**: Qwen3.5's Gated DeltaNet makes KV
  dramatically cheaper — 32K ctx fits where older 9B models managed 8K.
- MoE + expert offload trades VRAM for system RAM and speed (10-25 tok/s;
  needs ≥32GB RAM for 30B-A3B).
- Below Q4, instruction-following decays: smaller model at Q4 beats bigger at Q2.

## Local RAG stack (embeddings, rerankers, speech)

- **Embeddings** run via `/v1/embeddings`, not chat (cards carry
  `kind: embedding`): `qwen3-embedding` (quality/multilingual, selectable dims,
  instruction-aware) or `nomic-embed-text` (274MB, the ecosystem fast default) or
  `embeddinggemma` (code corpora); `bge-m3` (1.2GB, dense+sparse hybrid, 100+
  languages) is the multilingual alternate.
- **Rerankers**: `bge-reranker-v2-m3` is the production default — a normal
  cross-encoder. TRAP: **Qwen3-Reranker is NOT drop-in** — it's a causal-LM
  yes-token scorer under a specific chat template with open llama.cpp bugs; naive
  use via community Ollama tags silently returns garbage scores.
- **Speech is out of Ollama's scope entirely**: use faster-whisper
  (`whisper-large-v3-turbo`, ~6GB, multilingual default) or NVIDIA Parakeet for
  English. Gemma 4 E-variants accept audio *input*, but transcription pipelines
  still belong to the dedicated runtimes.

## Prompting differences (technique card: small-model-prompting)

Small/quantized models invert several frontier rules: in-prompt CoT helps
(non-reasoners), examples beat descriptions, one job per call, lower temperature
for structured work, always validate + retry JSON — or better, constrained
decoding (GBNF/structured outputs) where the runtime supports it. **Generation-
specific toggle lore**: Qwen3 honors in-prompt `/think` `/no_think`; **Qwen3.5
removed the soft switch** — toggle via runtime params only. R1-distills take NO
system prompt (all instructions in the user turn).

## Canonical sources & known-fake tags

Sources: **ollama.com/library** + **Hugging Face** (weights/GGUF). Circulating
SEO fabrications to exclude: "qwen3.5-coder:7b" (no such model — 404), "Llama 3.3
8B" (3.3 was 70B-only), "Mistral Small 3 7B" (Small 3 is 24B). Verify every tag
with `ollama list` / the live library page before carding.
