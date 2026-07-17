---
slug: multi-turn-context-shaping
name: Multi-turn context shaping
lever: context
helps: chat products and assistants that live for dozens of turns
hurts: single-shot pipelines
---
Chat history is a context you re-send every turn — shape it like one:
- **Freeze the system prompt; inject the dynamic** (user name, date, mode flags)
  into the message stream — interpolating them into the system prompt breaks the
  cache and re-anchors nothing.
- **Append-only + a moving cache breakpoint** on the newest turn; hits accrue as
  the conversation grows.
- **Prune the heavy middle first**: old tool results and pasted blobs are the
  bulk of most transcripts and the least load-bearing — clear or stub them
  (server-side context editing does this natively) before summarizing turns.
- **Handle the wall explicitly**: distinguish max_tokens (raise/stream) from
  context-window-exceeded (compact or split). Never silently truncate oldest
  turns — the model loses commitments made there; summarize into a pinned note.
- **Carry state, not transcript**: preferences, decisions, corrections that must
  survive belong in a structured note re-injected each turn (see session-memory).
