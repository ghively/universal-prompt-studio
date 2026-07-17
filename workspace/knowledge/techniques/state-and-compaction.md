---
slug: state-and-compaction
name: State files & fresh windows
lever: agentic
helps: tasks spanning multiple context windows or sessions (big builds, migrations, long research)
hurts: single-call prompts — irrelevant
---
For work longer than one context window, put state in the filesystem, not the
transcript: a structured feature/test list with pass-fail states (`tests.json` —
schema-shaped data the agent cannot declare "done" around while entries are
unfinished), freeform progress notes (`progress.txt`), and git as the checkpoint
log. Tell the agent tests are load-bearing: "It is unacceptable to remove or edit
tests."

**Fresh window beats compaction for milestone-shaped autonomous work with durable
artifacts** — current models are extremely good at rediscovering state from disk.
Compaction remains right for conversational continuity; match the tool to the
shape (see context-compaction). Be prescriptive on restart: "Review progress.txt,
tests.json, and the git log; run the integration test FIRST, before implementing
anything new." Session-start verification is load-bearing because prior-session
claims are untrusted — it catches regressions and false "done" claims that reading
the progress file misses. Use a different prompt for the *first* window (set up
the framework, write the tests, create init.sh) than for continuation windows
(work the todo list).

**Incremental-progress discipline**: one feature per session, and leave the tree
in a clean, mergeable state (commit or discard) before the window ends —
half-done state is what poisons handoffs.
