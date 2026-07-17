---
slug: mcp-server-design
name: MCP server design
lever: harness-artifacts
helps: building or configuring MCP servers whose tools agents will use
hurts: n/a
---
An MCP server is a prompt surface: its tool names, descriptions, and `instructions`
field are read by the model on every session. Design them as such:
- Tool descriptions follow tool-description-quality (what + when + when-not, third
  person, documented params with example values).
- The server `instructions` field is a mini system prompt: usage patterns, ordering
  constraints, cross-tool workflows — keep it lean; it loads always.
- Fewer, composable tools beat many overlapping ones: every extra tool is context
  cost and routing risk. A tool that wraps three operations with a mode param
  often routes better than three near-duplicate tools.
- Return structured, bounded results: a tool that dumps 50KB of JSON into context
  is a context-pollution bug; paginate/filter server-side.
- Reference tools in skills/instructions by qualified name (`Server:tool_name`).
