---
slug: skill-progressive-disclosure
name: Progressive disclosure
lever: skill-authoring
helps: any skill beyond a page; skills covering multiple domains
hurts: trivial one-page skills — just write the page
---
Three loading tiers: metadata (always in context) → SKILL.md body (loaded when
triggered) → bundled files (loaded only when needed, zero cost until read). Design
for the tiers: SKILL.md is a table of contents that points outward — "Form filling:
see FORMS.md" — not a textbook.

Rules that make it work:
- **References one level deep from SKILL.md** — nested reference chains get
  partially read (`head -100`) and information is silently lost.
- **Table of contents at the top of any bundled file > 100 lines**, so partial
  reads still reveal scope.
- **Split by domain** (`reference/finance.md`, `reference/sales.md`) so a sales
  question never loads finance schemas.
- Descriptive filenames (`form_validation_rules.md`, never `doc2.md`).
- Assume the model is already smart: cut every sentence explaining what it knows.
  Concise SKILL.md bodies measurably outperform verbose ones.

Two lifecycle facts change the economics after invocation:
- **Invoked content persists** for the rest of the session (the harness does not
  re-read the file per turn) — write standing instructions, not one-time steps.
  On auto-compaction only the first ~5,000 tokens of each skill survive, inside a
  ~25,000-token shared budget filled most-recent-first: put the load-bearing
  rules first in the body.
- **Subagent preloading inverts the tiers**: skills listed in a subagent's
  `skills:` field are injected in full at startup — on-demand disclosure doesn't
  apply there; keep preloaded skills short.
