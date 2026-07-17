# Design Language v2 — "Ideas, syntax-highlighted"

*Companion to PRODUCT.md. Live mockup: `docs/design/mockup.html`.
v2 replaces "Specimen & Margin" after owner feedback: too cardy, too generic,
wanted an idea-workspace vibe — direction chosen: dark IDE × bold expressive.*

## Concept

The whole app is **one continuous working session** — a lit terminal, not a
dashboard. There are no cards, no panels, no rounded rectangles: structure comes
from hairlines, indentation, line-number gutters, and type scale. Color does
semantic work the way an editor theme does — every hue has a job, nothing is
colored to look nice.

**Dark-only, by commitment.** This direction lives in one visual world. A light
variant, if ever wanted, is a second syntax theme ("daylight") designed on its own
terms — not an inversion.

## The theme — five hues, five jobs

| Hue | Hex | Job |
|---|---|---|
| **Amber (human)** | `#FFB454` | your asks, your answers, your actions |
| **Cyan (machine)** | `#5CCFE6` | payloads, model names, links, measured data |
| **Violet (coach)** | `#C792EA` | annotations, findings, observation |
| Green (pass) | `#86D97C` | verdicts from real runs only |
| Red (fail) | `#FF5F6B` | verdicts from real runs only |
| Ground / raise | `#0C0F14` / `#12161D` | blue-black, never pure black |
| Text / dim / faint | `#D8DDE6` / `#7E8899` / `#4C5666` | three text levels |
| Hairline | `#232A35` | all structure |

**State carries through luminance**: stale artifacts literally fade (reduced
opacity) until re-verified. Attention goes where the light is.

## Type — two voices

| Voice | Stack | Used for |
|---|---|---|
| Mono (default) | ui-monospace / SF Mono / Menlo | nearly everything — this is an IDE |
| Display sans | system sans at 800–850 weight, tight tracking, uppercase | big expressive moments: headlines, section titles |

Since the ground is mono-first, semantics move from typeface to **hue** (v1's
"mono means measured" rule is superseded by "hue is meaning"). The bold uppercase
display face is the expressive counterweight — used at large sizes only.

## Signature treatments

- **Ask mode is a scrollback.** Your ask is a big amber `>` line; the
  interrogation renders as inline dialogue (`?` question / `=` answer / `+`
  derived check in green); the compiled prompt is numbered source with the
  explanation **woven in as violet comment lines** (`# technique — why · card ›`),
  toggleable so the exact payload is always one click away.
- **The receipt is a diff.** Original-ask output as `−` lines, compiled-prompt
  output as `+` lines, checks in a `@` context line, verdict row with cost and a
  transcripts link.
- **The library is a tree**, not a card wall: status glyph + name + kind + mono
  stat per row; regressed rows go red, stale rows fade.
- **The coach writes a log**, not tips: `finding:` entries with evidence refs and
  a single amber `[ action ]`.
- **The model fingerprint is a text instrument**: `▮▮▮▮▮▮▮▮▮▯ .94` meter rows,
  stamped with battery version and measurement date.
- **Buttons are bracketed text**: `[ save to library ]` — amber, terminal-native.
- A slim **status bar** tops the app: workspace, attention count, month spend vs cap.

## Rules the UI refuses to break

1. **No cards.** A box must be a real container of state (a diff, a specimen) —
   never decoration.
2. **Hue is meaning.** Amber = you, cyan = machine, violet = coach, green/red =
   verdicts from real runs.
3. **Stale things fade.** Health is luminance.
4. **The session is the interface.** Everything reads top-to-bottom as one working
   document. No modal graveyards, no widget grids.

## Accessibility

Focus states in amber outline on all interactive text; hover targets get a
hue-dim background wash; contrast verified on the dark ground (dim text `#7E8899`
on `#0C0F14` ≈ 5.5:1); caret blink and all motion honor `prefers-reduced-motion`;
tabular numerals wherever digits align; wide code/diff content scrolls in its own
container.
