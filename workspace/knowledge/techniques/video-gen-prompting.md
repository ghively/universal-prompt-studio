---
slug: video-gen-prompting
name: Video generation prompting
lever: modality
helps: Veo 3.1, Kling 3.0, Runway Gen-4.5, Seedance 2.0 and current leaders
hurts: n/a — but note Sora 2 is DISCONTINUED (app dead 2026-04, API dies 2026-09); don't build on it
---
Write in **professional film grammar** — models adhere to camera vocabulary far
better than adjectives. The Veo formula: `[Cinematography] + [Subject] + [Action] +
[Context] + [Style & Ambiance]`; one shot = framing + camera move + subject + 2–3
action beats in order + lighting. Camera language is the highest-leverage lever:
dolly/tracking/crane/aerial/POV; wide/close-up/low-angle; shallow DoF/macro.

Advanced controls: **timestamp prompting** for multi-shot pacing
(`[00:00-00:02] Medium shot...`), reference images ("ingredients") for character
consistency — never textual re-description; audio is promptable on audio-native
models (dialogue in quotes, `SFX:` prefix, `Ambient noise:` prefix).

**Image-to-video is a different idiom**: describe MOTION ONLY, never re-describe
the image content. Negations: describe the absence positively ("a desolate
landscape with no buildings"), never "no X".
