# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
# Compile Tailwind CSS (one-shot)
npx tailwindcss -i ./src/input.css -o ./public/styles.css

# Compile with minification (matches what wrangler deploy runs)
npx tailwindcss -i ./src/input.css -o ./public/styles.css --minify

# Watch mode during development
npx tailwindcss -i ./src/input.css -o ./public/styles.css --watch

# Local dev server (Cloudflare Workers)
npx wrangler dev

# Deploy to Cloudflare
npx wrangler deploy
```

> `npm run <script>` may fail due to nvm shell init issues. Use `npx` directly instead.

## Architecture

Static games/experiment site deployed as a Cloudflare Workers asset site at games.farhaduneci.com.
No JavaScript framework or bundler — just HTML, vanilla JS, and Tailwind CSS.

- **`src/input.css`** — Tailwind v4 source. Color tokens and custom utilities only.
- **`public/`** — Everything served as static assets by Cloudflare. `styles.css` is compiled output.
- **`public/index.html`** — Games listing page. Lists all games in `public/html/`. Update manually when adding a new game.
- **`public/html/`** — Individual game/experiment HTML files. Each file is a standalone page.
- **`wrangler.jsonc`** — Wrangler config. The `build.command` runs Tailwind on every `wrangler deploy`.

## Adding a New Game

1. Drop the standalone HTML file into `public/html/`.
2. Add a corresponding entry in `public/index.html` (the games array at the top of the inline script).
3. Run `npx wrangler deploy` (or `npx wrangler dev` to preview locally).

## Tailwind Setup

Uses **Tailwind v4** (not v3). Key differences:
- `@import "tailwindcss"` instead of `@tailwind` directives
- `@source "../public/**"` restricts class scanning to the public directory
- `@custom-variant dark (&:where(.dark, .dark *))` — class-based dark mode
- CSS variable shorthand in HTML: `bg-(--bg)`, `text-(--text)`, etc.

The CLI is `@tailwindcss/cli` (separate package from `tailwindcss` in v4).
