---
slug: tool-description-quality
name: Tool description quality
lever: agentic
helps: any agent with tools — descriptions are the most under-evaluated prompt surface in agentic systems
hurts: nothing
---
The model chooses tools by reading descriptions; a weak description is a routing
bug. Rules:
- Description = **what it does + when to use it** (and when NOT to), in third
  person, with the key terms a task would contain.
- Document every parameter: meaning, format, an example value. Never leave the
  model to guess an ID format.
- Name by action (`create_invoice`, not `invoice_helper`); consistent naming across
  the toolset.
- MCP tools: reference with fully qualified names (`Server:tool_name`) in
  instructions.
- Current models overtrigger on shouty descriptions ("USE THIS WHENEVER...") —
  calm, precise conditions route better.
- Tool descriptions are testable artifacts: trigger tests (should-call /
  shouldn't-call) belong in the suite like any other check.
