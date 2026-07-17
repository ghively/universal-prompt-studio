---
slug: skill-library-curation
name: Multi-skill library curation
lever: skill-authoring
helps: any setup past ~10 skills; teams sharing a skill collection
hurts: n/a
---
The skill listing is budgeted, not free: ~1% of the context window, each
entry's description+when_to_use truncated at 1,536 chars, and on overflow the
least-invoked skills lose their descriptions first — an uncurated library
silently degrades its own triggering. Curate deliberately: front-load each
description's key use case, demote rare skills to "name-only" or "off"
(skillOverrides), and make human-only skills disable-model-invocation so they
cost nothing.

Overlap is a triggering bug, not redundancy insurance: two descriptions
matching the same task split or misroute selection. Give each skill a distinct
task territory, and add must-not-trigger tests against its neighbors'
territories, not just generic non-matches.

Name conflicts resolve by precedence — enterprise > personal > project, and
any of them override a bundled skill of the same name; plugin skills are
namespaced and cannot collide; nested monorepo skills get directory-qualified
names (`apps/web:deploy`). Audit with /doctor (listing cost and biggest
contributors) and /context (post-budget listing size).
