---
slug: skill-description-triggering
name: Skill description triggering
lever: skill-authoring
helps: the single highest-leverage lines in any skill — they decide whether the skill loads at all
hurts: n/a
---
Only the listing entry is pre-loaded — in Claude Code that is `description` plus
`when_to_use` (a separate frontmatter field), truncated at 1,536 chars per entry
inside a listing budget of ~1% of the context window. On overflow, least-invoked
skills lose their descriptions first — so **front-load the key use case in the
first sentence**. Write it as **what it does + when to use it**, with the
concrete trigger terms a task would contain:

Good: `Extract text and tables from PDF files, fill forms, merge documents. Use
when working with PDF files or when the user mentions PDFs, forms, or document
extraction.`
Bad: `Helps with documents.`

**Always third person** ("Processes Excel files...") — the description is
injected into the model's context, and first/second person causes discovery
problems.

The description is no longer the only trigger: `paths:` globs gate auto-loading
to matching files, and `disable-model-invocation: true` removes the description
from context entirely (human-only, zero listing cost). Trigger *precision*
matters as much as recall: a skill that loads when it shouldn't pollutes
context. Test both directions — should-trigger and must-not-trigger tasks (the
studio's wind tunnel runs exactly this), including against neighboring skills'
territories (skill-library-curation).
