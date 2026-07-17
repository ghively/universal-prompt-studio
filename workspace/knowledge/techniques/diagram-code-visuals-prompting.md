---
slug: diagram-code-visuals-prompting
name: Diagram & chart generation via code
lever: modality
helps: producing diagrams/visuals as Mermaid, SVG, DOT, or chart code
hurts: n/a — for pixel-image diagrams see image-gen; code stays the editable/versionable route
---
LLMs draw best through a DSL: **Mermaid first** (flowcharts, sequence, state,
ER), Graphviz/DOT for dense graphs, SVG or chart libraries when exact geometry
matters. Strict syntax suppresses drift — and the output is diffable and
versionable, which pixels are not.

The idiom: lock format before content ("Output only valid Mermaid, no prose"),
then enumerate nodes and relationships explicitly rather than narrating. For
15+ node diagrams, go two-step: comments-only skeleton, then syntax. The
reliability loop is **render → feed parser errors back → regenerate** — models
self-correct excellently against concrete error messages; wire a renderer in.
Constrain layout explicitly (direction, subgraphs, label caps); unconstrained
diagrams sprawl. State what to omit — negative constraints matter unusually
much here.
