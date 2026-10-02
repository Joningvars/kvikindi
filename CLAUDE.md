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

## Design system (Kvikindi v1, PestQuell-inspired)
- Tokens live in `src/tailwind.css` `@theme`: brand greens (`brand-50…950`; 500 = accent/icons, 600 = buttons/links for AA contrast), `ink`, `muted`, `mist` (soft card bg), `line`, `sale`; radii `rounded-card` (16px) / `rounded-panel` (24px); shadows `shadow-card` / `shadow-float`.
- Fonts are self-hosted in `assets/` (Fontsource, OFL) via `snippets/theme-fonts.liquid`: Red Hat Display (`font-display`, headings) + Inter (`font-sans`, body).
- Component classes (in `@layer components`): `page-width`, `section-y`, `eyebrow`, `h-display`, `h-section`, `lede`, `btn` + `btn-primary|secondary|white|ghost-light`, `card`, `chip`, `field`, `link-arrow`, `rte`, `details.accordion`.
- Icons: `{% render 'icon', icon: 'rat', class: 'size-6' %}` (Lucide, ISC). To add one, regenerate `snippets/icon.liquid` from `lucide-static`.
- Pest tags: product tags prefixed `meindýr:` (e.g. `meindýr:Mýs`) render as chips on product cards and product pages.
- Homepage sections: `hero` (trust-card blocks), `pest-grid` (pest blocks → collections), `featured-products`, `why-us`, `faq`, `cta-banner`; optional `testimonials` (real reviews only). Header group: `announcement-bar` + `header`.
- Default content is Icelandic; bracketed text like `[Settu inn …]` is placeholder copy that must be replaced before launch.
- Contact details (phone/email/address) come from Theme settings → Contact.
- Product page: per-option pill picker (radios + variants JSON in `sections/product.liquid`), variant image jumps to front, `@app` blocks supported. Recommendations come from `sections/product-recommendations.liquid` (Section Rendering API, Shopify's automatic recommendations).
- Collection page: storefront filters from Search & Discovery via `snippets/facet-filters.liquid` (desktop sidebar auto-submits; mobile uses a `<dialog>` drawer). Price filter uses whole currency units (`divided_by: 100`), which suits ISK.
- App blocks: `product`, `collection` and the generic `apps` section accept `@app` blocks.
- Services (Kvikindi is service-first, shop second): sections `service-hero`, `service-grid`, `process-steps`, `info-columns`, `service-request` (Shopify contact form, emails the store). Page templates: `page.thjonusta` (overview), `page.thjonusta-mys|geitungar|skordyr|fyrirtaeki`, `page.hafa-samband`. Pages are created in Shopify admin and assigned these templates; service links expect handles `thjonusta`, `mys-og-rottur`, `geitungar`, `skordyr`, `fyrirtaekjathjonusta`, `hafa-samband`.
- Placeholder photos `assets/kvikindi-hero-house.jpg` and `assets/kvikindi-why-kitchen.jpg` are AI-generated stand-ins used when no image is picked; replace with real photos before launch.
- Credentials section (`credentials`): certificate cards (text only — no third-party logos). Robert Arnarson's BASIS Principles of Insect Control cert (PIC/1293) is on `page.thjonusta` and `page.thjonusta-skordyr`. Original logo raster is kept in `src/brand/`; site uses traced SVGs `assets/kvikindi-logo.svg` (dark, compact) and `assets/kvikindi-logo-white.svg` (white, with tagline).
