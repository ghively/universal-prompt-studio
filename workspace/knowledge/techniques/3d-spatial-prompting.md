---
slug: 3d-spatial-prompting
name: 3D & spatial generation prompting
lever: modality
helps: text/image-to-3D assets (Meshy 6 class) and world models (Marble / World API)
hurts: n/a — young field; expect idiom churn faster than other modality cards
---
Two distinct disciplines. **Asset generation**: prompt like a game-asset spec,
not a scene — one object per prompt, form + materials + style target ("stylized
low-poly", "PBR game-ready"), stated intended use. Rigging and retopology live
in the pipeline; short concrete prompts beat cinematic paragraphs because
everything spatial must be inferable as geometry.

**World generation** (Marble / World API): describe a *navigable space* —
layout and spatial relationships first ("a narrow alley opening onto a plaza"),
then materials, lighting, era. Multimodal conditioning is the real control
surface: images, panoramas, or coarse 3D layouts anchor geometry better than
words; worlds are edited conversationally region by region. Exports (splats,
meshes) feed engines. [Prompt-length sweet spots and negation handling:
practitioner literature still thin — revisit within months.]
