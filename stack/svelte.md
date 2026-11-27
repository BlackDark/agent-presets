# Tech stack: Svelte

Used when Svelte was chosen (embedded UI, or an agreed deviation from React) or already exists. Svelte 5 runes only in new code.

- App: **SvelteKit** for full apps and multi-page SPAs; static hosting with `adapter-static`. Host without history fallback (device UI): hash router, `sveltekit({ router: { type: 'hash' } })` in `vite.config.ts` (SvelteKit 3; 2.x: `kit.router.type` in `svelte.config.js`, ≥ 2.14). It rejects `prerender`/`ssr` page options. Plain **Vite** + `@sveltejs/vite-plugin-svelte` only for one-page widgets or a recorded budget under ~30 KB.
- Components: **shadcn-svelte** (Bits UI) copied into the repo, Tailwind 4, `@lucide/svelte`. Add and use the registry component that covers the UI. A custom component in its place: a comment on that file names the official component and why it does not fit.
- Server state, polling, refetch after changes: `@tanstack/svelte-query`; with a Hey API client, generate the query options via its `@tanstack/svelte-query` plugin ([contracts.md](contracts.md)). Forms: `@tanstack/svelte-form`. Tables: `@tanstack/svelte-table` 9 with shadcn-svelte table components.
- i18n: **Paraglide JS** (SvelteKit integration).
- Reactive browser APIs (media queries, storage, debounce, intervals, element size): `svelte/reactivity`, then **runed** (Bits UI already depends on it).
- Types: `svelte-check` in `pnpm check`; `tsc` doesn't see `.svelte` files.
- Lint/format: Biome; its Svelte support covers script blocks only, markup is unchecked.
- Tests: Vitest + `@testing-library/svelte`; Playwright for E2E and screenshots.
- Everything else (validation, tooling, delivery): [core.md](core.md).
