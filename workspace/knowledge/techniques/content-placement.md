---
slug: content-placement
name: Content placement
lever: context
helps: prompts with long documents or variable content; recurring prompts (cache)
hurts: nothing
---
Order the prompt: stable instructions first, long/variable content last, and for very
long content restate the core ask in one line after it. Never bury an instruction in
the middle of a document.

**Mechanism**: models attend most reliably to the start and end of the window;
middles get lost. Separately, keeping the stable part as an unchanging prefix makes
provider prompt-caching work, cutting cost on recurring runs.
