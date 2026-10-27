# Your Project's Title...
Your project's description...

## Environments
- Preview: https://main--acme-labs--vadymdevhandler.aem.page/
- Live: https://main--acme-labs--vadymdevhandler.aem.live/

## Documentation

Before using the aem-boilerplate, we recommend you to go through the documentation on https://www.aem.live/docs/ and more specifically:
1. [Developer Tutorial](https://www.aem.live/developer/tutorial)
2. [The Anatomy of a Project](https://www.aem.live/developer/anatomy-of-a-project)
3. [Web Performance](https://www.aem.live/developer/keeping-it-100)
4. [Markup, Sections, Blocks, and Auto Blocking](https://www.aem.live/developer/markup-sections-blocks)

## Installation

```sh
npm i
```

## Linting

```sh
npm run lint
```

## Local development

1. Create a new repository based on the `aem-boilerplate` template
1. Add the [AEM Code Sync GitHub App](https://github.com/apps/aem-code-sync) to the repository
1. Install the [AEM CLI](https://github.com/adobe/helix-cli): `npm install -g @adobe/aem-cli` (or run it without installing via `npx -y @adobe/aem-cli up`)
1. Start AEM Proxy: `aem up` (opens your browser at `http://localhost:3000`)
1. Open the `acme-labs` directory in your favorite IDE and start coding :)

`aem up` serves your local code (blocks, scripts, styles) on top of the previewed content from the `.aem.page` site. Code changes reload in the browser automatically; content changes appear after you preview them in your authoring tool.

Useful `aem up` options:

| Command | What it does |
| --- | --- |
| `aem up --no-open` | Start without opening a browser window |
| `aem up --open /some/path` | Open a specific page on start |
| `aem up --port 3001` | Use another port |
| `aem up --url https://<branch>--acme-labs--vadymdevhandler.aem.page` | Fetch content from a different origin (e.g. another branch) |
| `aem up --no-livereload` | Turn off automatic browser reload |
| `aem up --forward-browser-logs` | Print the browser console in the terminal |
| `aem up --log-level debug` | Verbose server logging |

Run `aem up --help` for the full list.

To see the raw markup a block receives (before any JavaScript decoration), request the page's `.plain.html`:

```sh
curl http://localhost:3000/index.plain.html
```

## Commands

| Command | What it does |
| --- | --- |
| `npm i` | Install lint tooling (there is no build step) |
| `npm run lint` | Run ESLint and Stylelint (the same check CI runs on every push) |
| `npm run lint:fix` | Auto-fix lint issues |
| `npx eslint blocks/cards/cards.js` | Lint a single JS file |
| `npx stylelint blocks/cards/cards.css` | Lint a single CSS file |
| `aem up` | Run the local development server |
