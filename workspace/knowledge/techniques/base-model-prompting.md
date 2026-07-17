---
slug: base-model-prompting
name: Base-model (non-instruct) prompting
lever: model-fit
helps: completion-style use of base/pretrained checkpoints — FIM code completion, logprob classification, creative continuation, research probes
hurts: instruct/chat models — completion-style prompts waste their tuning
---
Base models continue documents; they do not answer people. Every instruct-era
habit fails here, and the failure is characteristic: an instruction gets
*continued* (or translated, or listed alongside nine similar instructions)
rather than obeyed.
- **No chat template, no system prompt, no roles** — the document is the whole
  interface. Applying a chat template to a base checkpoint is the inverse of
  the local chat-template bug: equally fatal, opposite direction.
- **Prompt by demonstration pattern**: write the opening of a document whose
  natural continuation is your desired output. Few-shot transcripts (k
  input→output pairs in the exact target format, then the real input and a
  format cue) are the core move — the model imitates the document's pattern.
- **Stop sequences are mandatory**: a base model doesn't know it's done; it
  generates the next fake example. Use your example delimiter as the stop.
- **First tokens set the register**: the model infers genre, quality, and
  style from the opening — start the document sounding like the output you
  want (a bulleted spec begets bullets; polished prose begets prose).
- **Logprobs over generation for classification**: score candidate
  continuations instead of parsing free text — cheaper and stricter.
- **Code base models**: use the family's FIM tokens verbatim from the model
  card; they are not portable across families.
Sampling matters more than on tuned models (no alignment mode-collapse):
expect to tune temperature/top_p per task, and guard against greedy loops.
