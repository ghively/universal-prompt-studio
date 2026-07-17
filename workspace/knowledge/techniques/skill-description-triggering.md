---
slug: skill-description-triggering
name: Skill description triggering
lever: skill-authoring
helps: the single highest-leverage line in any skill — it decides whether the skill loads at all
hurts: n/a
---
Only name+description are pre-loaded; Claude picks from potentially 100+ skills by
description alone. Write it as **what it does + when to use it**, with the concrete
trigger terms a task would contain:

Good: `Extract text and tables from PDF files, fill forms, merge documents. Use
when working with PDF files or when the user mentions PDFs, forms, or document
extraction.`
Bad: `Helps with documents.`

**Always third person** ("Processes Excel files...") — the description is injected
into the system prompt, and first/second person causes discovery problems.

Trigger *precision* matters as much as recall: a skill that loads when it shouldn't
pollutes context. Test both directions — should-trigger and must-not-trigger tasks
(the studio's wind tunnel runs exactly this).
