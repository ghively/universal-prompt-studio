---
slug: audio-music-gen-prompting
name: Audio & music generation prompting
lever: modality
helps: Suno (V5.5), ElevenLabs Music v2, and kin
hurts: n/a — but note Udio is download-frozen post-UMG settlement; don't build on it
---
Two-field discipline (Suno idiom): **Style field** = 6-12 comma descriptors (current consensus; <5 too vague, >15 redundant), FRONT-LOADED — earlier tokens weigh more, so genre first — using the
five-part formula — genre/subgenre + mood/energy + vocal character + key
instruments/production + tempo feel ("synth-pop" beats "pop"; "raspy male vocals"
beats "male vocals"). **Lyrics field** = structure in brackets: `[Intro] [Verse 1]
[Pre-Chorus] [Chorus] [Bridge] [Outro] [End]` — parameterized tags work
(`[Bridge: stripped down, piano only]`); vocal delivery cues inline in parentheses;
always end with `[End]` or songs trail off. Number verses (different melodies);
repeat identical chorus text for melodic repetition.

**What's ignored/filtered**: artist names (describe the sound instead); BPM is now
ATTEMPTED within ~±2-5 on v5.5 (lead with BPM + key + genre) but never locked;
negative text in Style remains unreliable ("no drums" — use the Exclude field),
technical mixing terms. **ElevenLabs Music v2** (the commercially-licensed option):
high-level use-case prompts work; `solo <instrument>`, `a cappella`,
"instrumental only", tempo and key cues supported; section-level editing.
