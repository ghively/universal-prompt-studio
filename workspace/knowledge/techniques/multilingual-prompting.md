---
slug: multilingual-prompting
name: Multilingual prompting
lever: model-fit
helps: any non-English or cross-lingual work
hurts: n/a
---
The old law "always prompt in English" is now **task-dependent** (35-language/
4-task studies): for extraction, NER, and classification over non-English content,
**matching prompt language to content language consistently wins**; for complex
reasoning, an English system prompt with an explicit output-language declaration
still holds. The declaration matters more than the prompt language — always state
"Respond only in <language>" separately.

Frontier models are near-parity on high-resource languages (single-digit
instruction-retention drop); **low-resource languages still show 30-point-class
gaps** — never assume, always eval per language. Keep JSON keys and schemas in
English even when prose output is target-language (format discipline degrades in
non-Latin scripts). Never machine-translate a battle-tested English prompt
verbatim — examples and register need native adaptation, and translation quality
is itself a measured performance driver.
