---
slug: mcp-server-design
name: MCP server design
lever: harness-artifacts
helps: building or configuring MCP servers whose tools agents will use
hurts: n/a
---
An MCP server is a prompt surface — but in the tool-search era, what loads when
has changed. Claude Code defers tool schemas by default: only tool *names* and the
server `instructions` field load at session start; descriptions load when the
model searches. Design consequences:
- The **`instructions` field is now the discovery surface** — it works like a
  skill description, telling the model when to search this server. It and each
  tool description are truncated at 2KB; keep both lean.
- Tool descriptions still gate routing at search time: follow
  tool-description-quality (what + when + when-not, third person, documented
  params with example values).
- Fewer, composable tools beat many overlapping ones: every extra tool is context
  cost and routing risk. A tool that wraps three operations with a mode param
  often routes better than three near-duplicate tools.
- Return structured, bounded results: harnesses cap tool output (Claude Code
  warns at 10k tokens, truncates at 25k by default) — paginate/filter
  server-side. Make validation failures *tool execution errors* whose text tells
  the model how to self-correct.
- Tools are one of three primitives — server prompts and resources are prompt
  artifacts too (see mcp-resources-and-prompts).
- Scoping option: attaching a server to a subagent (`mcpServers` in its
  frontmatter) keeps its tools out of the main conversation entirely.
- Reference tools by the harness's fully qualified form (Claude Code:
  `mcp__server__tool`).
