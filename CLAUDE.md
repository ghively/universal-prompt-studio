# CLAUDE.md — instructions for coding agents in this repository

This repository is **Prompt Studio**: a local-first prompt development environment.
It is fully specified but not yet built. Your job is to build it exactly as specified.

## The one rule

**The spec is the authority.** Everything you need is in `docs/spec/`. Do not invent
features, rename things, restructure directories, add dependencies, or "improve" the
design. If the spec and your instinct disagree, the spec wins. If the spec is genuinely
ambiguous, take the simplest literal reading and leave a `// SPEC-AMBIGUITY:` comment.

## How to work

1. Read `docs/spec/README.md` first (index + global conventions).
2. Work through `docs/spec/13-ACCEPTANCE.md` **top to bottom** — it is a checklist of
   phases (P0–P9). Never start a phase before the previous phase's acceptance tests pass.
3. For each checklist item: read the referenced spec section, implement it, run its
   acceptance test, check the box by editing 13-ACCEPTANCE.md, commit.
4. Commit after every green acceptance test with message `P<phase>: <item> (spec §<ref>)`.
5. All LLM prompts used by the app are given **verbatim** in `docs/spec/04-PROMPTS.md`.
   Copy them exactly. Do not paraphrase, shorten, or "fix" them.
6. All seed content (model cards, technique cards, practices) already exists under
   `workspace/`. Do not regenerate or rewrite it.

## Layout (fixed)

- `app/` — all application code (server + web UI). Exact scaffold in `docs/spec/01-STACK.md`.
- `workspace/` — user data and knowledge. The app reads/writes here. Never hardcode
  content that belongs in workspace files.
- `docs/` — vision, product, roadmap, design, spec. Never modify anything in `docs/`
  except checking boxes in `docs/spec/13-ACCEPTANCE.md`.

## Environment

- Node.js ≥ 22. API keys come from `app/.env` (see `docs/spec/01-STACK.md`);
  never commit `.env`, never log key values.
- Every provider call must go through the budget guard and run cache
  (`docs/spec/03-API.md` §5–6). No exceptions, including internal pipeline calls.

## Verification

- `cd app && npm run check` (typecheck) and `npm test` must pass before any commit.
- UI work must match `docs/design/mockup.html` — it is the visual ground truth.
  Token values in `docs/spec/05-UI.md` are exact; copy, don't approximate.
