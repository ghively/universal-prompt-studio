---
slug: tool-description-quality
name: Tool description quality
lever: agentic
helps: any agent with tools — descriptions are the most under-evaluated prompt surface in agentic systems
hurts: nothing directly — but verbose descriptions across a large toolset are always-loaded context cost and degrade routing
---
The model chooses tools by reading descriptions; a weak description is a routing
bug. Rules:
- Description = **what it does + when to use it** (and when NOT to), in third
  person, with the key terms a task would contain.
- Document every parameter: meaning, format, an example value. Never leave the
  model to guess an ID format.
- Name by action (`create_invoice`, not `invoice_helper`); consistent naming across
  the toolset — prefix-namespace related tools (`github_list_prs`), and consolidate
  frequently chained operations into one tool.
- MCP tools: reference them by the harness's fully qualified form (Claude Code:
  `mcp__server__tool`) in instructions.
- Current models overtrigger on shouty descriptions ("USE THIS WHENEVER...") —
  calm, precise conditions route better.
- **Tool responses are co-equal prompt surface**: token-efficient outputs with
  pagination/truncation defaults, human-readable identifiers over opaque IDs, and
  error messages that steer ("this error string should tell the agent what to call
  instead") — half of error recovery is tool design (error-recovery-discipline).
- Tool descriptions are testable artifacts: trigger tests (should-call /
  shouldn't-call) belong in the suite like any other check.
