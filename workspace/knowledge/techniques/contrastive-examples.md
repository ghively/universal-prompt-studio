---
slug: contrastive-examples
name: Contrastive examples
lever: examples
helps: tasks with a common failure mode you can name; grader/judge prompts
hurts: simple tasks; risk of teaching the bad pattern if the "bad" example is more vivid than the "good"
---
Show a good output **and** a bad output, labeled, with one line on why the bad one
fails. This teaches the boundary, not just the center, of acceptable output.

**Intuition** (house theory — consistent with provider grader guidance, no direct
study): a single positive example leaves the failure direction undefined; the
contrast pins it.

**Use**: "GOOD (grounded): ... / BAD (invented detail): ... — the March date appears
nowhere in the source."
