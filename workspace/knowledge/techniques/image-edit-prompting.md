---
slug: image-edit-prompting
name: Image-editing prompting (instruct-edit)
lever: modality
helps: instruction-based editing on Nano Banana Pro/2, GPT Image 2, FLUX.2 edit modes
hurts: n/a — but generation-style scene descriptions actively fight the base image
---
Editing is a different discipline from generation: the base image exists, so the
prompt's job is **what changes and what stays**. Never re-describe the whole
scene — that invites regeneration. State the change as an instruction ("replace
the blue sofa with a leather armchair"), then pin invariants explicitly: "keep
the lighting, camera angle, and everything else identical." Preservation
language is the control surface, not politeness.

**Semantic masking**: current editors take a text-defined mask — name the region
("only the sky", "the text on the sign") instead of painting pixels. For
recurring characters, supply reference images and say what must match ("same
face, same jacket"); Nano Banana 2 is the consistency leader. Expect
multi-turn: chained small edits beat one giant edit prompt. For text edits,
quote the exact replacement string.
