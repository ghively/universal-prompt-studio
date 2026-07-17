---
slug: format-sensitivity
name: Format sensitivity
lever: structure
helps: eval design; migrating a prompt between models; choosing markdown vs JSON vs XML framing
hurts: nothing — this card is a warning label, not a technique
---
Formatting is a measured confound, not cosmetics. The same prompt as plain text /
markdown / YAML / JSON moves some models up to 40% on code tasks; the best format
for one model transfers poorly to another (markdown tended to win on GPT-4-class,
JSON on smaller). FormatSpread: trivial choices — delimiter, casing, spacing —
spread up to 76 accuracy points across templates.

**Consequences**: A/B two prompts only at fixed formatting — never credit a
wording change measured across a format change. Model migration must re-test
format, not just wording (idiom is a card fact: XML on Claude, markdown on
GPT-lineage). Pick one delimiter convention per prompt and never mix —
consistency beats any particular choice.
