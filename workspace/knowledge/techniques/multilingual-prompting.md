---
slug: multilingual-prompting
name: Multilingual prompting
lever: model-fit
helps: any non-English or cross-lingual work
hurts: n/a
---
The old law "always prompt in English" no longer holds universally — but neither
does its replacement. Recent multi-language studies split: English prompts still
win on many tasks (pretraining dominance), native-language prompts win on
culturally grounded tasks, and at least one study found prompt/content-language
matching helped not at all. The defensible rule is **task- and
language-pair-dependent: eval per pair**. What is settled: the output-language
declaration matters more than the prompt language — always state "Respond only in
<language>" separately, and **restate it near the end for long outputs**, because
the documented failure mode is mid-output drift back to English (worst on
extractive tasks and long generations; weak models sometimes translate the
instruction instead of answering it).

Frontier models are near-parity on high-resource languages; low-resource languages
still show large gaps — never assume, always eval per language. Language strength
also differs sharply **per model family**: it is a card fact — check the target's
card or probe it. Keep JSON keys and schemas in English even when prose output is
target-language (format discipline degrades in non-Latin scripts). Never
machine-translate a battle-tested English prompt verbatim — examples and register
need native adaptation, and translation quality is itself a measured performance
driver.

**Tokenizer economics**: non-Latin and low-resource scripts cost roughly 2-4x
tokens per word. That silently shrinks effective context, inflates cost and
latency, and accelerates context rot — budget accordingly before blaming the
model. **Cross-lingual retrieval** (query in language A over documents in
language B) is the most common real mixed-language task: state both languages
explicitly, decide which one the answer should be in, and expect extraction
quality to track the *document* language's support, not the query's.
