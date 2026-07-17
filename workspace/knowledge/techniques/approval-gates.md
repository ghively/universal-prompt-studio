---
slug: approval-gates
name: Approval gates
lever: agentic
helps: agents with side-effecting tools (sends, spends, deletes, deploys), especially downstream of untrusted input
hurts: read-only agents — pure friction
---
Gate by **action class**, not by turn. Per-turn permission prompts train
rubber-stamping — reported harness telemetry puts permission-prompt approval
around 93%; a gate that always opens is not a gate. Reserve human confirmation
for the irreversible and the high-blast-radius: external communications,
spending, destructive operations, and any consequential action taken after
reading untrusted content.
In the prompt, name the classes and the protocol: "Before any <gated action>,
stop and present: the intended action, its target, why, and how it would be
undone. Wait for explicit approval." Make the request *legible* — a diff, a
dry-run output, a recipient list — never a bare "OK to proceed?". A reviewed
plan is the cheapest gate of all: one plan-approval before execution replaces
many mid-flight interrupts.
Enforce with hooks where 100% matters (hook-vs-prompt); prompted gates are for
judgment calls a hook can't classify. And design the blocked path: the agent
should stage the gated action and continue other work, not idle.
