---
slug: image-gen-prompting
name: Image-generation prompting
lever: modality
helps: prompting current image models — GPT Image 2 (#1 arenas), Nano Banana Pro/2 (Gemini 3 image; editing leader), FLUX.2 Pro, Midjourney V8 (NOTE: Imagen 4 deprecated, shutdown 2026-08)
hurts: n/a — but note it's the OPPOSITE of 2022-era tag-stuffing
---
Modern image models are language-native: **describe the picture in flowing natural
language**, the way you'd brief a photographer — subject, action, setting, light,
mood, composition, medium — in one coherent paragraph. The 2022 idiom (comma tag
walls, "masterpiece, 8k, trending on artstation", negative-prompt rituals) is dead
weight or harm on current models.

What still earns its place: concrete nouns over adjectives ("weathered brass
diving helmet" beats "highly detailed helmet"); explicit style/medium naming;
composition and lens vocabulary when it matters ("low angle, 35mm, shallow depth
of field"); exact quoted text for any in-image text (current models render text —
spell it and say where); aspect ratio as a parameter, not prose. Iterate by
*editing the description*, not appending qualifiers — appended fragments dilute
the coherent scene description that these models parse best.

**2026 shifts**: GPT Image 2 *reasons* (and can search) before drawing — complex
briefs can be stated as requirements rather than painted descriptions, at ~109s/
image cost. Character consistency is a reference-image workflow ("keep the face
and jacket identical"; Nano Banana 2 leads). Midjourney remains a live different
dialect (parameters, srefs). Editing is a different idiom entirely — see
image-edit-prompting.
