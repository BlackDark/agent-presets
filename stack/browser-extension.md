# Tech stack: browser extensions

- Build: Manifest V3, **React**, **Vite**, `@crxjs/vite-plugin`, Tailwind, Biome. Same Vite major as the rest of the repo.
- Browsers: Chrome, Edge, Firefox from one code base; `BROWSER=firefox` selects the Firefox build (`build:firefox`).
- Texts: `_locales/{de,en}/messages.json` via `chrome.i18n` (stores read name and description from there). Paraglide not used here.
- Validation: size-budget rule in [core.md](core.md) for popups and content scripts.
- Permissions: minimal; optional host permissions requested on use; no tracking; ship a `PRIVACY.md`.
- Tests: Vitest for logic; Playwright on popup/options pages with a `chrome-stub.ts` (also screenshots).
- Store material (icons, banners, screenshots, texts): `store-assets/`, outside `public/` so it isn't packaged.
- Release: tag `v<version>` builds per-browser zips and attaches them to the GitHub release; store upload (Chrome draft, Firefox with source archive, Edge) in the same workflow, secrets per store.
