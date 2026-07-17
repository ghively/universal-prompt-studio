# Design Language v3 — "Night Studio"

*Companion to PRODUCT.md. Live mockup: `docs/design/mockup.html`.
v3 keeps v2's semantic color system (the part that worked) and replaces the
terminal world with an **idea canvas**: v1 was too cardy, v2 too console.*

## Concept

A dark **idea canvas** — the dot grid of a thinking tool (whiteboard/canvas apps),
not the gutter of a code editor. Work floats on the canvas in soft washes of light;
nothing is boxed with borders, nothing is terminal chrome. Hierarchy comes from
scale, hue, and space. Dark-only, by commitment — a light variant would be a second
world designed on its own terms.

## The palette (unchanged from v2)

| Hue | Hex | Job |
|---|---|---|
| **Gold (you)** | `#F0C674` | asks, answers, actions (all primary buttons) |
| **Cyan (machine)** | `#5CCFE6` | payloads, model names, links, measured data |
| **Violet (coach)** | `#C792EA` | annotations, findings, observation |
| Green (pass) | `#86D97C` | verdicts from real runs only |
| Red (fail) | `#FF5F6B` | verdicts from real runs only |
| Ground | `#0D1017` + dot grid `rgba(216,221,230,.055)` | the canvas |
| Text / dim / faint | `#DCE1EA` / `#8B95A6` / `#566173` | three text levels |

Hues appear as **washes** (`rgba(hue, .07–.09)` fills, radius 18–20px, soft
shadow) and **glows** (blurred radial fields, glow dots) — never as borders.
**State carries through luminance**: stale artifacts fade until re-verified.

## Type

| Voice | Stack | Used for |
|---|---|---|
| Display | `ui-rounded, "SF Pro Rounded", system-ui` at 700–800, tight tracking, mixed case | headlines, section titles, the ask itself, note titles |
| Body | system sans | everything conversational |
| Mono | ui-monospace stack | **payloads and measurements only** — exactness is the message |

No uppercase-terminal styling; the rounded display face is the personality.

## Signature treatments

- **The ask is the headline.** Your words render as the largest type on the
  screen, in gold — the tool's voice never upstages yours (rule R4).
- **Interrogation as typeset conversation**: dim question, indented gold answer,
  small green "check minted: …" line under answers that derive checks.
- **The compiled prompt is a sheet on the canvas** (cyan wash, no line numbers)
  with numbered violet pins; the coach's notes float beside it as glow-dot notes
  with display-face titles. Hover/focus either side and its partner lights
  (violet wash + inset edge on the span; glow on the note).
- **The receipt is before/after**: two washes — red-tinted "your ask, as written"
  and green-tinted "the compiled prompt" — same input, per-check ✓/✕ lines,
  tally row with cost and a transcripts link.
- **The library is a shelf**: borderless rows, glow-dot status, stale rows fade.
- **Coach findings are pinned notes**: violet washes with a ±0.5° tilt (straightened
  under `prefers-reduced-motion`), evidence line, one gold pill action.
- **Model fingerprints are gauges**: thin glowing cyan bars, stamped with battery
  version and measurement date.
- **Buttons are gold pills** (filled = primary with soft glow; ghost = secondary).

## Rules the canvas refuses to break

1. **No chrome.** No borders, gutters, sigils, or status bars. Washes and light only.
2. **Hue is meaning.** Gold you / cyan machine / violet coach; green-red verdicts only.
3. **Stale things fade.** Health is luminance.
4. **Your words lead.** The ask is set larger than anything the machine says back.

## Accessibility

Gold focus outlines on all interactive elements; note/pin cross-highlighting works
on focus, not just hover; contrast held at ≥4.5:1 for body text on the canvas;
tilt and glow effects disabled under `prefers-reduced-motion`; tabular numerals for
aligned digits; payload sheets scroll horizontally in their own container.
