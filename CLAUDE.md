# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

@AGENTS.md

## Commands

- `npm i` — install lint tooling (the only dependencies; nothing is bundled or built).
- `npm run lint` — ESLint (airbnb-base) + Stylelint. This is the only CI check (`.github/workflows/main.yaml`, runs on every push).
- `npm run lint:fix` — autofix both.
- Single file: `npx eslint blocks/cards/cards.js`, `npx stylelint blocks/cards/cards.css`.
- `npx -y @adobe/aem-cli up` — local server at `http://localhost:3000` serving local code over previewed content.
- There is no test suite.

ESLint requires the `.js` extension on every import (`import/extensions`) and Unix line endings.

## Page lifecycle (`scripts/scripts.js`)

`head.html` loads `scripts/aem.js` and `scripts/scripts.js` as modules. `loadPage()` runs three phases:

1. **Eager** — `decorateMain(main)` (icons → auto blocks → sections → blocks → buttons), then loads only the first section and waits for its first image (LCP). Fonts load here only on desktop or if already cached this session.
2. **Lazy** — header, remaining sections, footer, `styles/lazy-styles.css`, fonts.
3. **Delayed** — `scripts/consent-check.js`. It is a stub CMP: consent defaults to declined, so `scripts/consented.js` (where analytics/martech belong) loads only with `?consent=accept`. It fires a `consent.update` window event either way.

`decorateMain` is also used by `blocks/fragment/fragment.js` on fetched `{path}.plain.html`, so fragments go through the same decoration pipeline.

## Auto blocks and content conventions

- Links to `/fragments/...` are replaced inline with the fragment's sections.
- Links to `/widgets/...` become a `widget` block (whole paragraph if the link is alone in it).
- A link alone in a paragraph becomes a button only when authored with formatting: bold → `.button.primary`, italic → `.button.secondary`, both → `.button.accent`.

## Widget block (`blocks/widget/`)

A widget link `/widgets/{dir}/{name}.html?foo=bar` loads `widgets/{dir}/{name}.html`, `.css` and `.js` from the code base (not from content). Before loading, the block is re-classed: `.widget` gets class `{name}` and loses `.block`, `.widget-wrapper`/`.widget-container` are renamed to `{name}-wrapper`/`{name}-container`, query params go to `dataset`, and the link href goes to `data-source`. The widget's JS default export receives the block element. So widget CSS is scoped to `.{name}`, not `.widget`. No `widgets/` directory exists yet.

## Security constraints

`head.html` sets a strict CSP (`script-src 'nonce-aem' 'strict-dynamic'`, `require-trusted-types-for 'script'`). `scripts.js` installs a default Trusted Types policy that strips `<script>` elements from `createContextualFragment`/`document.write` input and removes `srcdoc` from iframes. As a result:

- Load third-party scripts with `loadScript`/dynamic `import()` from already-trusted code. Inline `<script>` in injected HTML will not run.
- `innerHTML` assignments pass through the default policy; do not rely on embedded scripts or `srcdoc` iframes surviving.

## Docs lookup

- Search `www.aem.live` for Edge Delivery questions; generic "AEM" results are usually about the Java/OSGi stack and don't apply here.
- `https://www.aem.live/llms.txt` lists the key docs. Any aem.live page is also served as markdown by appending `.md` (e.g. `https://www.aem.live/developer/markup-sections-blocks.md`).
- The `docs-search` skill (from the project's `aem-edge-delivery-services` plugin) searches aem.live directly.

## Repo notes

- `.hlxignore` keeps dotfiles, `*.md`, `package*.json` and `test/*` from being served; add any other non-site files there.
- `blocks/hero/hero.js` is intentionally empty (CSS-only block).
- The PR template's test URLs point at `.aem.live`; AGENTS.md requires an `.aem.page` branch link — include the `.aem.page` one.
