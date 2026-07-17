---
slug: mcp-resources-and-prompts
name: MCP resources & prompts
lever: harness-artifacts
helps: designing MCP servers beyond tools — server-side prompt templates and referenceable context
hurts: n/a
---
Tools are only one of three MCP server primitives. The other two are prompt
artifacts in their own right:
- **Prompts** are server-shipped prompt templates. In Claude Code they surface as
  `/mcp__server__promptname` commands with parsed arguments, injected directly
  into the conversation. Author them like slash commands: complete brief, explicit
  arguments, output shape — but versioned and deployed *with the server*, so every
  client of the server gets the same vetted prompt. Use a server prompt when the
  workflow belongs to the system the server fronts (e.g. "triage this ticket"),
  a local command when it belongs to the user's repo.
- **Resources** are addressable context: users pull them with `@server:uri`
  mentions and they attach like files. Design URIs to be listable and
  fuzzy-searchable; return bounded, focused content — a resource is a
  content-placement decision, not a dump endpoint.
Prompts and resources move curation server-side: one team maintains the prompt,
every session benefits. Smoke-test them like any prompt artifact.
