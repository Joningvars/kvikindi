# CLAUDE.md

Custom Shopify theme (Online Store 2.0: JSON templates + sections + theme blocks), Liquid + Tailwind CSS v4, developed with Shopify CLI. Based on Shopify's Skeleton theme (pre-v2, sections-based).

## Conventions
- Pages are composed in the theme editor: `templates/*.json` list sections; real markup lives in `sections/*.liquid` with a `{% schema %}` (settings + presets) so merchants can add/reorder/edit without code.
- Do NOT use the `{% block %}` / `{% partial %}` tags or Liquid-only templates from Skeleton v2: they aren't supported on this store and fail to upload.
- `templates/*.json` and `config/settings_data.json` are also written by the theme editor; `npm run pull` before editing them to avoid overwriting merchant changes.
- Visible strings go through `{{ 'key' | t }}` (locales/). Use `routes.*` for storefront URLs; never hardcode `/cart` etc.

## Tailwind
- Source `src/tailwind.css`; output `assets/tailwind.css` is generated (never hand-edit) but committed, because Shopify's GitHub integration deploys the repo as-is. Run `npm run css:build` before committing.
- Scanned paths: `layout/`, `templates/`, `sections/`, `blocks/`, `snippets/`, `assets/*.js`. Add an `@source` line in `src/tailwind.css` if classes live elsewhere.

## Tooling
- Store/environment config is in `shopify.theme.toml` (`development` environment); scripts pass `-e development`.
- `npm run dev` (Tailwind watch + `shopify theme dev`), `npm run check` (Theme Check), `npm run push` (unpublished theme).
- Never push to or publish the live theme from the CLI; use `--unpublished` and publish from Shopify admin.
