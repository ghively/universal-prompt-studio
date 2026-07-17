---
slug: session-memory
name: Cross-request session memory
lever: context
helps: products where the same user returns; long-lived agents
hurts: stateless one-shot APIs — pure overhead
---
The API is stateless; memory is a product feature you build. Current patterns:
- **Files-as-memory is the native primitive**: Claude's memory tool
  (client-executed /memories directory) — models are explicitly trained for file
  memory. Format discipline: one lesson per file, one-line summary on top,
  update-don't-duplicate, delete when wrong.
- **Structured KV beats prose blobs**: store decisions, preferences,
  corrections, open threads as small keyed records — inject only relevant keys,
  delete bad entries without rewriting history.
- **Inject at session start**, after the system prefix, inside the cached region.
- **Write policy matters more than read policy**: capture explicit corrections
  and stated preferences; don't auto-memorize everything. Never store secrets —
  memory replays verbatim into every future context.
- **Security**: validate memory paths (traversal via model-supplied paths),
  per-user directories, and treat re-injected memory as untrusted input — a
  poisoned memory is a persistent prompt injection.
