---
slug: voice-realtime-prompting
name: Voice & realtime agent prompting
lever: modality
helps: speech-to-speech agents (gpt-realtime-2.1 family, ElevenLabs Agents, Gemini Live, Nova Sonic)
hurts: n/a — but a text-agent prompt ported unchanged actively degrades speech agents
---
Speech prompts have their own physics. OpenAI's official skeleton: 8 short labeled
sections (Role & Objective / Personality & Tone / Context / Reference
Pronunciations / Tools / Rules / Conversation Flow / Safety & Escalation) — short
bullets, CAPS for critical rules, conditional logic in plain words. Specify
per-turn length ("2–3 sentences per turn") and add an explicit variety rule —
these models over-follow sample phrases and turn robotic. Pin the output language.

Speech-specific musts: an unclear-audio rule ("if unintelligible, ask them to
repeat" — even the word choice matters; "unintelligible" measurably beat
"inaudible"); numbers as words (digits mispronounce); tool parameters documented
with written-format examples because transcripts contain spoken forms ("john at
gmail dot com"); preamble phrases to mask tool latency; reasoning effort LOW for
production voice (never extended-reason on unclear audio — clarify instead).
Don't prompt turn-taking/interruption on platforms where it's config-level.
Start minimal; add rules only for observed failures.
