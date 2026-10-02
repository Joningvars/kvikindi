# Shopify theme

A custom Shopify theme built with Liquid, Tailwind CSS v4 and Shopify CLI.
It's based on Shopify's official [Skeleton theme](https://github.com/Shopify/skeleton-theme), which is MIT licensed (see `LICENSE.md`).

## First-time setup

Requires Node 22.12+ and Git.

```bash
cd ~/development/shopify-theme
npm install          # installs Shopify CLI, Tailwind, etc. locally (no global install needed)
npm run login        # opens the browser to log in to Shopify
```

Then put your store's address in `shopify.theme.toml`:

```toml
[environments.development]
store = "your-store.myshopify.com"
```

## Daily work

```bash
npm run dev
```

This runs Tailwind in watch mode and `shopify theme dev` side by side. Open the
local preview URL it prints (usually http://127.0.0.1:9292). Liquid, CSS and JS
changes hot-reload. Nothing touches the live theme.

| Command | What it does |
|---|---|
| `npm run dev` | Tailwind watch + local preview against the store |
| `npm run check` | Theme Check (Liquid lint, performance, accessibility) |
| `npm run push` | Builds CSS and uploads as a new **unpublished** theme |
| `npm run pull` | Pulls the theme from the store (e.g. settings changed in the theme editor) |

Any Shopify CLI command also works through npx, e.g. `npx shopify theme list -e development`.

## Structure

```
layout/      theme.liquid (page shell), password.liquid
templates/   one Liquid file per page type: the composition root for each page
blocks/      reusable building blocks, rendered with {% block 'name' %}
snippets/    small partials, rendered with {% render 'name' %}
assets/      CSS, JS, images (served from Shopify's CDN)
config/      theme settings schema + saved values
locales/     translations (every visible string goes through | t)
src/         source files that get built into assets/ (Tailwind entry)
```

Read `AGENTS.md` for the Skeleton theme's conventions. The main rules: no
`sections/`, no JSON templates, all CSS/JS lives in `assets/`, and visible text
goes through `{{ 'key' | t }}`.

## Tailwind

- Edit `src/tailwind.css`, never `assets/tailwind.css`. That file is generated.
- Commit the built `assets/tailwind.css`. Shopify's GitHub integration deploys
  whatever is in the repo, so the built file has to be there. Run
  `npm run css:build` before committing.
- Tailwind's preflight reset loads after `base.css`. Trim `base.css` as you
  replace it with utilities.

## Deploying

1. Push this repo to GitHub.
2. In Shopify admin, go to Online Store → Themes → Add theme → Connect from GitHub and pick the branch.
3. Work on feature branches and preview them as separate themes. Publish from the admin when ready.
