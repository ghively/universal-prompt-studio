---
slug: motivation-context
name: Motivation context
lever: clarity
helps: any rule that looks arbitrary without its reason; rules the model keeps half-following
hurts: nothing — one clause of why is cheap
---
Give the reason behind a rule and the model generalizes correctly: "Never use
ellipses — the text-to-speech engine can't pronounce them" beats "NEVER use
ellipses." The why lets the model handle cases the rule didn't enumerate (it will
also avoid em-dash asides, for the same TTS reason, unprompted).

**Mechanism**: a bare rule constrains one behavior; a motivated rule teaches the
underlying objective, which constrains the whole family of behaviors.
