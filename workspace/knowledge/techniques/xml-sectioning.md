---
slug: xml-sectioning
name: XML sectioning
lever: structure
helps: Claude-family targets; any prompt with 3+ distinct parts (role, rules, context, format)
hurts: models whose idiom is markdown (use headers there instead — same lever, different syntax)
---
Wrap each part of the prompt in named tags: <role>, <instructions>, <context>,
<output_format>. Reference sections by name ("follow <output_format> exactly").

**Mechanism**: explicit boundaries stop instruction bleed — rules read as rules, data
reads as data, and injected content inside <context> is less likely to be obeyed as
instructions. Claude models are specifically trained on this convention.
