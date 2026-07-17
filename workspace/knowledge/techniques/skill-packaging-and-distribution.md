---
slug: skill-packaging-and-distribution
name: Skill packaging & distribution
lever: skill-authoring
helps: shipping skills beyond your own machine — teams, marketplaces, the API
hurts: quick personal experiments — a folder in ~/.claude/skills is enough
---
Four distribution tiers, in order of ceremony: commit `.claude/skills/` to the
repo (team-shared, short names); managed settings (org-wide); a plugin
(`skills/` dir + `.claude-plugin/plugin.json`) via a marketplace — plugin
skills become `/plugin-name:skill-name`, immune to name conflicts; the Skills
API (zip upload ≤ 30 MB, immutable versions, pin `version` or `latest`, max 8
skills per request, sandbox has no network access).

Package as a plugin when the skill needs companions — agents, hooks, MCP/LSP
servers, default settings all bundle — or when consumers shouldn't edit it.
Plugin versioning is real: set `version` in plugin.json, or every git commit
counts as a release. Anthropic runs two marketplaces (curated official;
reviewed community) plus git-repo marketplaces anyone can host privately.

Portability: the Agent Skills open standard (agentskills.io) is the common
subset — name + description frontmatter, relative Unix paths, scripts/
references/assets. Declare environment needs in `compatibility`; keep
Claude-Code-only frontmatter (context, hooks, paths) out of skills meant to
travel. Validate before shipping: `skills-ref validate` (spec) and
`claude plugin validate` (marketplace review runs the same check).
