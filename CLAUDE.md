# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Quick Start

```bash
# No build step. Open the HTML file directly in a browser:
start universal-prompt-studio-v11.html   # Windows
open universal-prompt-studio-v11.html    # macOS
xdg-open universal-prompt-studio-v11.html # Linux
```

No npm, no node_modules, no bundler. Everything is a single self-contained HTML file (~2950 lines).

## Architecture

**Single-file React app** loaded via CDN:
- React 18.3.1 + ReactDOM (production UMD builds, pinned versions)
- Babel Standalone 7.26.9 (in-browser JSX compilation via `<script type="text/babel">`)
- Tailwind CSS 3 (CDN, configured with `darkMode: 'class'`)

### Code Layout (inside the HTML file)

_(Line numbers are approximate — grep for the `const NAME =` / `function NAME(` anchors rather than trusting exact lines.)_

| Lines (approx) | Section |
|---|---|
| 35–47 | `MEDIUM_AESTHETICS` — 10 artistic mediums × 15 aesthetic keywords each |
| 48–127 | `IMAGE_SCHEMA` — image fields incl. `reference.*` (consistency) and `sd_local.*` (Stable Diffusion) sections |
| 128–149 | `VIDEO_SCHEMA` — extends IMAGE_SCHEMA with motion/audio/resolution/transitions (strips `sd_local.*`) |
| 150–183 | `INDUSTRY_SKILLS` — 25+ domains with top-10 skill arrays |
| 184–258 | `LLM_SCHEMA` — role, task, context, output, behavior, safety fields |
| 259–359 | `DEV_SCHEMA` — project vision through devops/security/docs |
| 360–489 | `MARKETING_SCHEMA` — campaign strategy through market research |
| 490–547 | `VIBE_SCHEMA` — vibe coder project builder (14 tech stack decisions from The Vibe Coder's Handbook) |
| 548–579 | `AUDIO_SCHEMA` — music/voice/SFX prompts (Suno, Udio, ElevenLabs) |
| 580–612 | `AGENT_SCHEMA` — tool-use & multi-agent prompts (Agent SDK, MCP) |
| 613–701 | `FRONTEND_SCHEMA` — frontend/website design prompts (visual style, layout, typography, motion, tech stack) |
| 702–853 | `SCHEMAS`, `SELECT_SENTINELS`, `SENTINEL_*`, `TYPE_META`, `SECTION_INFO` — registry and UI metadata |
| 854–1430 | `PRESETS` — one-click presets per prompt type |
| ~1431–1540 | Toast notification system + `useEscapeKey`/`useDarkMode` hooks + utilities |
| ~1541–1853 | `ChainBuilder` — multi-step pipeline component |
| 1854–2940 | `UniversalPromptStudio` — main component (forms, output, templates, field search, confirm modal) |
| ~2941–2954 | `App` wrapper + ReactDOM render |

## Key Patterns

### Schema-Driven Forms
All UI is generated dynamically from schema objects (`IMAGE_SCHEMA`, `LLM_SCHEMA`, etc.). Each field has:
- `type` — input type (`text`, `textarea`, `select`, `multiselect`, `checkbox`, `number`, `medium_aesthetics`)
- `section` — groups fields into collapsible accordion sections
- `condition` — optional conditional visibility (e.g., `'text.enabled'`)
- `default`, `options`, `placeholder`, `min`, `max`, `step`

The `SCHEMAS` object maps prompt types to their schemas. `SECTION_INFO` provides titles/icons/descriptions per section. `TYPE_META` stores mode metadata (icon, title, color gradient, paste target).

### Dot-Path Keys
Schema keys use dot notation (`'subject.hair_color'`, `'meta.aspect_ratio'`). These are stored flat in `formData` state — **not** nested.

### Output Generation Pipeline
`buildNestedJSON(formData)` (in `UniversalPromptStudio`, memoized on `promptType`) converts the flat dot-path `formData` into nested JSON, skipping empty values, internal `_`-prefixed keys, **and fields whose `condition` is currently unmet** (so hidden conditional fields don't leak into the output). Sentinel values like `'ask_me'` are resolved via `resolveSentinel()` (module scope). The result is memoized as `generatedJSON`. `jsonToPlainText(jsonStr)` flattens nested JSON back into `key › subkey: value` lines for the plain-text output mode. The UI toggles between these via `outputMode` (`'json'` | `'text'`).

### localStorage Persistence
- **Theme**: `promptStudioTheme` — `'light'` | `'dark'` | `'system'`
- **Templates**: `promptStudioTemplates` — `{ [name]: { type, data, timestamp } }`
- **Auto-save**: `promptStudioAutosave` — `{ type, data, timestamp }` (expires after 24h)
- **Chains**: `promptStudioChains` — saved chain pipelines

All writes go through `safeLocalStorageSet()` which catches quota errors.

### Toast Bus
A lightweight pub/sub event bus (`toastBus`) decoupled from the React tree. Call `showToast(message, type)` from anywhere. The `ToastContainer` component subscribes via `useEffect`.

### Dark Mode
`useDarkMode()` hook returns `[isDark, mode, setMode]`. Manages the `dark` class on `<html>` and syncs with `prefers-color-scheme` when mode is `'system'`. Persists to localStorage.

### Chain Builder
A separate component (`ChainBuilder`) for multi-step prompt pipelines. Steps reference prompt types from the main schemas. Includes "translate" steps that push output to 23+ platform targets (Canva, Figma, GitHub, Vercel, n8n, etc.).

## How to Extend

### Adding a New Field to an Existing Schema
1. Add an entry to the relevant schema (e.g., `IMAGE_SCHEMA`):
   ```js
   'section.field_name': { type: 'select', label: 'My Field', options: [...], default: '...', section: 'section_name' }
   ```
2. If the section already exists in `SECTION_INFO`, the field appears automatically.
3. If adding a new section, add an entry to `SECTION_INFO[type]` with `title`, `icon`, `desc`.

### Adding a New Prompt Type
1. Define a new schema constant (e.g., `AUDIO_SCHEMA`).
2. Add it to the `SCHEMAS` object.
3. Add metadata to `TYPE_META` (icon, title, desc, color gradient, pasteTarget).
4. Add section info to `SECTION_INFO`.
5. Optionally add presets to `PRESETS`.

### Adding a Preset
Add an entry to `PRESETS[type]`:
```js
'Preset Name': { 'field.key': 'value', 'another.key': 'value' }
```

## Conventions

- **No build tools** — all changes are made directly in the HTML file.
- **No external JS/CSS files** — everything is inline.
- **CDN versions are pinned** — update with care, test in-browser.
- **Prefer `useCallback`/`useMemo`** for functions and derived data in the main component to avoid re-render overhead in a 2000+ line single-component tree.
- **Toast for user feedback** — use `showToast()` instead of `alert()`.
- **Safe storage writes** — always use `safeLocalStorageSet()` for localStorage writes.
