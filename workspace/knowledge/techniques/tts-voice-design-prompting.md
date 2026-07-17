---
slug: tts-voice-design-prompting
name: TTS & voice-design prompting
lever: modality
helps: expressive TTS (Eleven v3 family and kin) and describe-a-voice generation
hurts: n/a — but SSML-era markup habits are dead weight on prompt-native models
---
The SSML era gave way to prompt-native direction. Eleven v3 idiom: **audio tags
in square brackets inline with the script** — emotions ([excited], [whispers]),
delivery, even SFX ([applause]) — placed exactly where the action happens.
Punctuation and capitalization are performance cues. The stability setting
trades adherence for expressiveness: creative/natural settings are required for
tags to fire fully. Multi-speaker dialogue scripts with per-speaker tags.

**Voice design** is a casting brief: age, gender, accent, texture, pace,
attitude, recording quality ("middle-aged New Yorker, gravelly, close-mic
studio quality") — specific beats generic, and audio-quality terms measurably
shift fidelity. For cloning, direction happens through script and tags, not
the clone prompt. Numbers, abbreviations, and URLs still need spelling out —
the oldest TTS rule survived the era change.
