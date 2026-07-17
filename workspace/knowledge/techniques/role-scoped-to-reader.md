---
slug: role-scoped-to-reader
name: Role scoped to the reader
lever: clarity
helps: recurring tasks where tone/detail must stay stable; outputs consumed by a specific person or program
hurts: nothing, but a generic "helpful assistant" role is dead weight
---
Name the role by **who consumes the output**, not by flattery. "You are a
customer-operations analyst writing summaries a CRM script consumes" fixes tone,
detail level, and format pressure in one sentence; "you are a helpful expert" fixes
nothing.

**Mechanism**: the model infers dozens of small decisions (jargon level, length,
hedging) from who it's told it is and who is reading. Scoping the role to the reader
makes those inferences consistent across runs.

**Before**: "You are a helpful assistant. Summarize this call."
**After**: "You are a support-operations analyst. Your summary is parsed by software,
so follow the schema exactly."
