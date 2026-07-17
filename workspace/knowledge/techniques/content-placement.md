---
slug: content-placement
name: Content placement
lever: context
helps: prompts with long documents (20k+ tokens) or variable content
hurts: nothing
---
Current guidance (tested up to ~30% response-quality gains on long inputs): put
**longform data at the top**, wrapped in `<documents><document index="n">` tags with
source metadata — and put your **query/instructions at the end**, after the content.
Never bury an instruction mid-document.

**Mechanism**: models attend most reliably to the edges of the window, and a query
adjacent to generation anchors the task after the model has read the material.

**Cache nuance**: the stable system prompt stays first (unchanging prefix = cache
hits); then documents; the variable query last. For 100k+ inputs, restate the core
ask in one line at the very end.
