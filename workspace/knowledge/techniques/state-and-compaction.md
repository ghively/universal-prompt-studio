---
slug: state-and-compaction
name: State files & fresh windows
lever: agentic
helps: tasks spanning multiple context windows or sessions (big builds, migrations, long research)
hurts: single-call prompts — irrelevant
---
For work longer than one context window, put state in the filesystem, not the
transcript: a structured state file (`tests.json` — schema-shaped data), freeform
progress notes (`progress.txt`), and git as the checkpoint log. Tell the agent tests
are load-bearing: "It is unacceptable to remove or edit tests."

**Fresh window beats compaction** for current models — they are extremely good at
rediscovering state from disk. Be prescriptive on restart: "Review progress.txt,
tests.json, and the git log; run the integration test before implementing anything
new." Use a different prompt for the *first* window (set up the framework, write the
tests, create init.sh) than for continuation windows (work the todo list).
