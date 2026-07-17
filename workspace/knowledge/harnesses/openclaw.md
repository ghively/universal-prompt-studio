---
slug: openclaw
name: OpenClaw (personal agent gateway)
memory_files: [AGENTS.md, SOUL.md, USER.md, IDENTITY.md, TOOLS.md, HEARTBEAT.md, MEMORY.md, memory/YYYY-MM-DD.md daily notes]
artifact_kinds: [workspace markdown files, heartbeat prompts, skills]
---
The breakout personal-agent harness (MIT, OpenClaw Foundation; formerly
Warelay/Clawdbot/Moltbot): one agent, ~20 messaging channels (WhatsApp, Telegram,
Slack, Discord, iMessage...), and a **prompt surface that is entirely plain
markdown files** injected in a fixed order — AGENTS.md (operating rules), SOUL.md
(persona/tone/boundaries), USER.md (who the user is), IDENTITY.md, TOOLS.md
(guidance only — does not control availability), HEARTBEAT.md, MEMORY.md. Per-file
truncation ~20k chars, ~60k total: budget like a skill listing, not an essay.

**Memory model**: MEMORY.md = curated durable facts, loaded every session
(truncated in context, intact on disk); daily notes load today+yesterday on reset
and are otherwise retrieved via memory_search/memory_get tools — write durable
facts to MEMORY.md, event logs to daily notes, and don't expect old notes in
context.

**Heartbeat = scheduled prompting**: a periodic agent turn (default 30m) driven by
a verbatim default prompt that reads HEARTBEAT.md and replies HEARTBEAT_OK when
idle. HEARTBEAT.md is therefore a standing-instruction artifact with the same
discipline as any recurring prompt: checkable conditions, explicit
do-nothing path, no accumulating task lists.

**Security reality check**: 2026 brought a one-click RCE (CVE-2026-25253),
135k+ exposed instances, and a malicious skill doing prompt injection — the
lethal-trifecta warning (least-privilege-tooling) applies harder here than
anywhere: this harness combines private data, untrusted inbound messages, and
external comms BY DESIGN. Gate the third leg.
