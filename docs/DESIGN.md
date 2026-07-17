# Design Language — "Specimen & Margin"

*Companion to PRODUCT.md. Live mockup: `docs/design/mockup.html` (Ask mode, receipt,
library, coach digest, model fingerprint).*

## Concept

The product's soul is **text under examination**, so the UI borrows from the two
worlds that are genuinely its own:

- the **annotated manuscript** — margin notes, editorial typography, generous
  measure, explanation beside the artifact;
- the **lab instrument** — mono readouts, calibrated meters, graph-paper restraint,
  numbers you can trust.

The signature visual: the prompt is a **specimen** — an exact payload bound for a
machine, set in monospace on cool paper — while everything human (explanations,
questions, coaching) lives in the margins in editorial type. Hovering a numbered
section of the specimen lights its margin note, and vice versa. The annotated
specimen *is* the curriculum.

## Tokens

| Token | Light | Dark | Role |
|---|---|---|---|
| paper | `#F3F5F7` | `#12161D` | ground (cool, biased toward accent) |
| surface | `#FCFDFE` | `#1A202A` | cards, panels |
| ink | `#1B2430` | `#E5EAF2` | text |
| ink-soft / ink-faint | `#536070` / `#8A96A4` | `#9BA8B9` / `#6B7889` | secondary / captions |
| **cobalt (accent)** | `#2B50C4` | `#8FA7FF` | interaction + annotation, nothing else |
| pass | `#2E7D4F` | `#5CBA8C` | verdicts only |
| stale/warn | `#A87708` | `#D9A83F` | verdicts only |
| regressed/fail | `#B3383E` | `#E0707A` | verdicts only |

Both themes are first-class; tokens are defined on `:root`, redefined under
`prefers-color-scheme: dark`, and again under `[data-theme=…]` so an explicit toggle
beats the OS preference. A faint accent-tinted engineering grid appears in the
masthead/header zones only.

## Type — three voices

| Voice | Stack | Used for |
|---|---|---|
| Serif · narrative | Iowan Old Style / Palatino / Georgia | headings, explanations, coach findings, interrogation questions |
| Sans · interface | system-ui stack | controls, labels, body UI |
| Mono · evidence | ui-monospace / SF Mono / Menlo | **payloads and measurements only**: prompts, costs, checks, model IDs, versions |

The serif/mono contrast carries the identity: human voice vs. machine payload.
In the build, ship a real webfont pairing with the same roles (candidates: a
bookish text serif + a neutral grotesque + a crisp mono), self-hosted — no CDN fonts.

## Rules the UI never breaks

1. **Mono means measured.** If it's monospace, it was counted, not written. Never
   set decorative text in mono.
2. **Semantic color is earned.** Green/amber/red appear only as verdicts from real
   runs. Decoration never borrows them; the accent never judges.
3. **Margins teach.** Explanation sits beside the artifact — never in modals or
   tooltip graveyards.
4. **Calm by default.** Nothing pulses, spins, or slides unless state actually
   changed. Motion is a reporting tool; `prefers-reduced-motion` is honored.

## Layout

- Left icon rail (64px): ASK · LIB · COACH · MODELS · LAB · QUEUE. Active item gets
  accent + soft fill.
- Content column max ~1000px; running text ≤ 65ch.
- Ask screen anatomy: ask bar (serif — your words are narrative) → interrogation
  Q&A rows, each showing its **derived check** on the right (the "your answers
  become tests" mechanic made visible) → evidence chips (mono pills) → the
  specimen/margin two-column grid → the receipt.
- Receipt: two equal columns (your ask vs. compiled prompt), per-check ✓/✕ rows,
  full-width verdict strip linking to raw transcripts.
- Library rows: status dot (health) + name + kind chip + mono stat. State reads at
  a glance without reading.
- Model panels: curated notes and measured fingerprint are visually separated;
  meters carry battery version + measurement date. "Measured, not asserted."

## Component inventory (v0.1 needs only these)

ask bar · interrogation row (+ derived-check tag) · evidence chip · specimen block
(+ numbered segment) · margin note (+ model-why footer) · receipt column ·
check row · verdict strip · status dot · kind chip · panel/panel-header ·
meter row · primary button · rail item · toast.

## Accessibility

Visible focus states everywhere (specimen segments and notes are focusable and
cross-highlight on focus, not just hover); contrast checked on both themes;
tabular numerals for all aligned digits; wide content scrolls in its own container.
