# itsaydrian.dev

Personal website for [Aydrian Howard](https://itsaydrian.dev), built with Astro and deployed on Cloudflare Workers.

## Tech Stack

- **[Astro 6](https://astro.build)** — static site framework with MDX support
- **[Cloudflare Workers](https://workers.cloudflare.com)** — SSR runtime via `@astrojs/cloudflare` adapter
- **[Tailwind CSS v4](https://tailwindcss.com)** — utility-first styling via `@tailwindcss/vite`
- **[Bun](https://bun.sh)** — package manager and script runner

## Development

```bash
bun install          # Install dependencies
bun run dev          # Start local dev server
bun run preview      # Build + run via Wrangler (simulates CF Worker)
bun run build        # Production build (outputs to dist/)
bun run deploy       # Build + deploy to Cloudflare Workers
```

## License

MIT © Aydrian Howard
