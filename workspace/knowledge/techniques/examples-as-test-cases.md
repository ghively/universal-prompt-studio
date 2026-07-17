---
slug: examples-as-test-cases
name: Examples as test cases
lever: examples
helps: any prompt with examples and a suite; keeping examples honest as models and specs drift
hurts: nothing directly — the cost is suite-maintenance discipline
---
Every example in a prompt is a claim: "given this input, this output is correct."
Claims get tested. Maintain each in-prompt example as a held-IN case in the eval
suite: run the prompt's own examples as inputs (example block removed for that
run) and assert the model reproduces an acceptable output.

**What this catches**: stale examples after a spec change (the most common silent
prompt rot); examples the current model no longer needs (zero-shot passes →
deletion candidate per zero-shot-first); examples that were never right.

**Discipline**: one source of truth — examples live in data files that the prompt
template and the suite both read, never pasted twice. When a suite case fails
repeatedly and the fix is "show it," promote it into the prompt as an example;
when a prompt example passes zero-shot for two model generations, demote it back
to the suite.
