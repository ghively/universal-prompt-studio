---
slug: image-gen-prompting
name: Image-generation prompting
lever: modality
helps: prompting current image models (gpt-image, Imagen, FLUX, Gemini image models)
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
