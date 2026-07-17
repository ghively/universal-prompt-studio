# Model catalog — the two canonical sources

Complete machine-readable snapshots of what exists. Curated cards (`../<id>.yaml`)
are written only for models worth prompting deliberately; everything else is still
selectable from the catalog.

| File | Source | Refresh |
|---|---|---|
| `openrouter.json` | `GET https://openrouter.ai/api/v1/models` (no key needed) | automated — the watcher re-fetches daily (spec 10-CURRENCY) |
| `bedrock.json` | https://docs.aws.amazon.com/bedrock/latest/userguide/model-cards.html | reviewed manually — HTML page, no stable API |

`fetched` dates stamp staleness. New ids vs. the previous snapshot become queue
proposals ("new model available"); drafting a curated card stays a human-approved
step (CARD_DRAFT flow).
