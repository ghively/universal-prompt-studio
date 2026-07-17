---
slug: claude-agent-sdk
name: Claude Agent SDK (headless)
memory_files: [whatever you configure — none by default]
artifact_kinds: [system prompts, tool definitions, skills (loadable), subagent definitions]
---
**What it injects**: only what you pass. The SDK gives you Claude Code's agent loop
(tools, file access, MCP) with a configurable system prompt — closer to bare metal
than the CLI, but with the harness's tool-use scaffolding intact.

**Prompting implications**:
- You own the system prompt: identity, standing rules, and output contracts go
  there, written once (system-vs-user-placement applies fully).
- No automatic memory files — if you want CLAUDE.md-like behavior, you load it.
- Headless runs are this studio's wind-tunnel runner: deterministic setup makes
  scenario evals reproducible.
- Budget and turn caps are yours to set — persistence prompting (agentic-persistence)
  interacts directly with max_turns.
