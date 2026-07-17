---
slug: recency-and-cutoff-management
name: Knowledge-cutoff & recency management
lever: context
helps: anything touching current events, prices, versions, dates, live systems
hurts: purely closed-book creative tasks
---
The model's world ends at its training cutoff; your product's doesn't.
- **State the date — placed for cache**: models don't reliably know "today."
  Inject the current date once, in the message stream (a date in the cached
  prefix invalidates it daily).
- **Search-first for time-sensitive classes**: enumerate them ("recent events,
  current prices/roles/versions — search before answering from memory"); current
  models are high-precision/low-recall on deciding to search, and the
  instruction recovers recall.
- **Name the cutoff when no tools exist**: "Your knowledge ends ~<date>; for
  anything after, say so instead of guessing."
- **Timestamp retrieved context** and prefer newer on conflict — otherwise a
  2023 doc and a 2026 doc are equally authoritative text.
- **Version-pin technical answers** ("answer for <library> vX").
