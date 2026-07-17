# Prompt Studio

A local-first **prompt development environment**: you describe what you need done, it
interrogates you briefly, compiles a model-aware prompt, **explains every choice**, and
**proves** the result is better on real runs. Over time it watches how you actually use
AI and coaches you to get better — while keeping its model knowledge current.

**Status: specified, not yet built.** This repository currently contains the complete
blueprint and all seed content. There is no application code yet — it is written to be
built, mechanically, from the spec.

## Read in this order

| Document | What it is |
|---|---|
| [`docs/VISION.md`](docs/VISION.md) | Architecture rationale and the five goals |
| [`docs/NORTHSTAR.md`](docs/NORTHSTAR.md) | The end-product pitch |
| [`docs/PRODUCT.md`](docs/PRODUCT.md) | Concrete end-state: data model, screens, jobs, modes |
| [`docs/ROADMAP.md`](docs/ROADMAP.md) | Milestones M0–M9 with exit criteria |
| [`docs/DESIGN.md`](docs/DESIGN.md) + [`docs/design/mockup.html`](docs/design/mockup.html) | The locked "Night Studio" visual language |
| [`docs/spec/`](docs/spec/) | **The build spec** — mechanical, end-to-end, no judgment calls |
| [`workspace/`](workspace/) | Seed content: model cards, technique cards, practices, config |

## For builders (human or agent)

Start at [`docs/spec/README.md`](docs/spec/README.md) and follow
[`docs/spec/13-ACCEPTANCE.md`](docs/spec/13-ACCEPTANCE.md) phase by phase. `CLAUDE.md`
carries the standing rules. The spec is the authority; do not invent scope.

## License

MIT — see [LICENSE](LICENSE).
