# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
# Development
bun run dev          # Start Astro dev server

# Build & Preview
bun run build        # Build for production (outputs to dist/)
bun run preview      # Build + run via Wrangler locally (simulates CF Worker)

# Deploy
bun run deploy       # Build + deploy to Cloudflare Workers

# Generate Cloudflare type bindings
bun run cf-typegen
```

> Note: `package.json` uses `pnpm`-style scripts but the repo has switched to `bun` (see `bun.lock`). Use `bun run <script>`.

## Architecture

This is a personal website (`itsaydrian.dev`) built with **Astro 5** and deployed as a **Cloudflare Worker** via the `@astrojs/cloudflare` adapter.

### Key design points

- **SSR via Cloudflare adapter** — `astro.config.mjs` configures the `cloudflare` adapter with `imageService: "cloudflare"`. The v13 adapter uses `@cloudflare/vite-plugin` directly, providing a workerd-based dev/preview runtime (near-exact replica of production). `Astro.locals.cfContext` gives access to the `ExecutionContext`; Cloudflare env vars are accessed via `import { env } from 'cloudflare:workers'`.
- **Tailwind CSS v4** — integrated via `@tailwindcss/vite` Vite plugin (no `tailwind.config.*` file; config is inline in `astro.config.mjs`). Global styles live in `src/styles/global.css`.
- **Wrangler config** — `wrangler.jsonc` targets the custom domain `itsaydrian.dev`, sets the worker entry to `@astrojs/cloudflare/entrypoints/server` (pre-built by the adapter; do not point at dist/), and serves static assets from `./dist`.
- **No test suite** is configured yet.

### Source layout

```
src/
  pages/       # File-based routing (index.astro = homepage)
  layouts/     # Shared HTML shell (Layout.astro)
  components/  # Reusable Astro components (Welcome.astro)
  styles/      # global.css (Tailwind import + base styles)
  assets/      # Static SVGs
  env.d.ts     # Astro/Cloudflare type references
```
