# CLAUDE.md

Custom Shopify theme (Liquid + Tailwind CSS v4), developed with Shopify CLI.
Based on Shopify's Skeleton theme. Its conventions are in AGENTS.md and are binding:

@AGENTS.md

## Project additions
- Tailwind v4: source is `src/tailwind.css`; output `assets/tailwind.css` is generated (never hand-edit) but committed, because Shopify's GitHub integration deploys the repo as-is. Run `npm run css:build` before committing.
- Tailwind scans `layout/`, `templates/`, `blocks/`, `snippets/`, `assets/*.js`. Add an `@source` line in `src/tailwind.css` if classes live elsewhere.
- Store/environment config is in `shopify.theme.toml` (`development` environment). Scripts pass `-e development`.
- Commands: `npm run dev` (Tailwind watch + `shopify theme dev`), `npm run check` (Theme Check), `npm run push` (unpublished theme).
- Never push to or publish the live theme from the CLI; use `--unpublished` and publish from Shopify admin.
