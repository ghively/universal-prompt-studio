---
slug: least-privilege-tooling
name: Least-privilege tooling & blast radius
lever: agentic
helps: any agent with side-effecting tools; mandatory when inputs include untrusted content
hurts: nothing — this is the capability-side security baseline
---
Injection defense (untrusted-content-quarantine) limits what attacker text can
*say*; blast-radius design limits what a compromised agent can *do*. Size the
blast radius before writing the prompt: what is the worst a fully adversarial
model could do with exactly these tools and credentials? Then shrink it:
- **Minimum tool set per task and per phase** — a research phase gets no write
  tools; mask tools between phases rather than granting the union up front.
- **Scoped, short-lived credentials** — repo-scoped tokens over org-wide,
  just-in-time over persistent.
- **Sandbox the side effects** — container not host, branch not main, staging
  not prod; make the cheap-to-undo path the only path available.
- **Least agency** — constrain not just access but action rate and reach; the
  lethal-trifecta rule: private data + untrusted content + external comms in one
  agent is an exfiltration machine — split the trifecta across agents or gate
  the third leg (approval-gates).
Prompted directives ("never touch prod") are defense-in-depth only — an injected
agent ignores them. The allowlist and the sandbox are the wall; the prompt line
just reduces accidents (hook-vs-prompt).
