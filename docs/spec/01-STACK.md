# 01 — Stack, layout, scaffold

## 1. Directory layout (exact)

```
app/
├── package.json
├── tsconfig.json
├── vite.config.ts
├── .env.example
├── server/
│   └── src/
│       ├── index.ts          # entry: starts Hono server on PORT (default 5680)
│       ├── routes/           # one file per route group: ask.ts, artifacts.ts, models.ts,
│       │                     #   runs.ts, instruments.ts, config.ts, observe.ts, coach.ts,
│       │                     #   suites.ts, arena.ts, queue.ts, battery.ts
│       ├── providers.ts      # provider adapter (03-API §3)
│       ├── llm.ts            # llmCall() wrapper: budget + cache + run log (03-API §4-6)
│       ├── prompts.ts        # verbatim prompt constants (04-PROMPTS)
│       ├── checks.ts         # check executor (06-CHECKS)
│       ├── workspace.ts      # all workspace file IO
│       ├── schemas.ts        # zod schemas for every data shape (02-DATA)
│       └── util.ts           # hash(), usd conversions, id(), now()
├── web/
│   ├── index.html
│   └── src/
│       ├── main.tsx
│       ├── App.tsx           # router
│       ├── api.ts            # typed fetch client for every endpoint
│       ├── tokens.css        # design tokens copied exactly from 05-UI §1
│       ├── components/       # 05-UI §4 component list, one file each
│       └── pages/            # Ask.tsx, Library.tsx, Artifact.tsx, Models.tsx,
│                             #   Coach.tsx, Queue.tsx, Lab.tsx
└── (tests colocated as *.test.ts)
workspace/                    # data — already seeded, formats in 02-DATA
```

## 2. package.json (verbatim; do not add or upgrade without SPEC-AMBIGUITY note)

```json
{
  "name": "prompt-studio",
  "private": true,
  "type": "module",
  "scripts": {
    "dev": "concurrently -n server,web \"tsx watch server/src/index.ts\" \"vite\"",
    "build": "vite build && tsc -p tsconfig.json --noEmit",
    "start": "NODE_ENV=production tsx server/src/index.ts",
    "check": "tsc -p tsconfig.json --noEmit",
    "test": "vitest run"
  },
  "dependencies": {
    "@hono/node-server": "^1.13.0",
    "gray-matter": "^4.0.3",
    "hono": "^4.6.0",
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "react-router-dom": "^6.26.0",
    "yaml": "^2.5.0",
    "zod": "^3.23.0"
  },
  "devDependencies": {
    "@types/node": "^22.0.0",
    "@types/react": "^18.3.0",
    "@types/react-dom": "^18.3.0",
    "@vitejs/plugin-react": "^4.3.0",
    "concurrently": "^9.0.0",
    "tsx": "^4.19.0",
    "typescript": "^5.6.0",
    "vite": "^5.4.0",
    "vitest": "^2.1.0"
  }
}
```

Notes:
- JSON-schema validation for the `json_schema` check type is implemented with zod-built
  validators generated at runtime from a **restricted** schema dialect (06-CHECKS §2.2) —
  no ajv dependency.
- No provider SDKs. Provider calls are plain `fetch` with exact shapes (03-API §3).

## 3. tsconfig.json (verbatim)

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "Bundler",
    "jsx": "react-jsx",
    "strict": true,
    "skipLibCheck": true,
    "noEmit": true,
    "types": ["node", "vite/client"]
  },
  "include": ["server/src", "web/src"]
}
```

## 4. vite.config.ts (verbatim)

```ts
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  root: 'web',
  plugins: [react()],
  server: {
    port: 5173,
    proxy: { '/api': 'http://localhost:5680' }
  },
  build: { outDir: '../dist', emptyOutDir: true }
});
```

In production (`npm start`), the server serves static files from `app/dist` for any
non-`/api` path, falling back to `index.html` (SPA).

## 5. .env.example (verbatim; user copies to .env)

```
# At least ANTHROPIC_API_KEY is required. Others enable more models.
ANTHROPIC_API_KEY=
OPENROUTER_API_KEY=
OPENAI_API_KEY=
PORT=5680
WORKSPACE_DIR=../workspace
```

Server startup: if `ANTHROPIC_API_KEY` is missing, log
`FATAL: ANTHROPIC_API_KEY is required` and exit(1). Missing optional keys mean models
on those providers return `provider_error` with message `no API key configured for <provider>`.

## 6. Workspace bootstrap

On startup, ensure these directories exist (create if missing):
`artifacts/`, `models/`, `models/catalog/`, `knowledge/techniques/`, `knowledge/harnesses/`, `knowledge/probes/`, `runs/`, `runs/cache/`,
`runs/observed/`, `verdicts/`, `journal/`, `queue/`, `evals/`.
Never overwrite existing files during bootstrap.
