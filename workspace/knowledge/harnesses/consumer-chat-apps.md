---
slug: consumer-chat-apps
name: claude.ai & ChatGPT as harnesses
memory_files: [project instructions, custom instructions / personal preferences, managed memory]
artifact_kinds: [Projects, custom styles / personality presets, Custom GPTs, uploaded skills]
---
The consumer chat apps are harnesses too — layered prompt surfaces the coach
should recognize when sessions arrive from them:

**claude.ai**: Projects = instructions + knowledge (RAG-retrieved on paid tiers,
"up to 10x" capacity) with **project-scoped memory**; account-wide personal
preferences; custom styles (from a writing sample or description); Skills
(uploaded zips, same SKILL.md anatomy as Claude Code — skill cards apply).
Memory is viewable/editable by category, pausable, and excluded in incognito.

**ChatGPT**: custom instructions (1,500 chars free / 5,000 paid) + personality
presets stack with **two memory mechanisms** — saved memories (explicit,
individually deletable) and chat-history reference (implicit); Projects exist on
all tiers with per-project instructions and optional project-only memory; Custom
GPTs (instructions + 20 files + Actions) remain the shareable-artifact path.

**Prompting implications**:
- Project instructions are the CLAUDE.md of the consumer tier: same lean, calm,
  standing-rules discipline; per-chat repetition is a smell the coach flags.
- Memory is an uncurated context source: instructions the user forgot they wrote
  keep firing — audit it when behavior drifts (same failure shape as a stale
  rules file).
- The persona layers (styles/presets) stack UNDER your prompt — a "formal" preset
  plus a casual-tone instruction is a conflict the model resolves invisibly;
  align or disable the preset before debugging the prompt.
