# Tech stack: Svelte

Used when Svelte was chosen (embedded UI, or an agreed deviation from React) or already exists. Svelte 5 runes only in new code.

- App: **SvelteKit** for full apps; plain **Vite** + `@sveltejs/vite-plugin-svelte` for a static SPA (embedded, widgets). Static hosting: SvelteKit `adapter-static`.
- Components: **shadcn-svelte** (Bits UI) copied into the repo, Tailwind 4, `@lucide/svelte`.
- Server state: `@tanstack/svelte-query`. Forms: `@tanstack/svelte-form`. Tables: shadcn-svelte data table over `@tanstack/table-core`.
- i18n: **Paraglide JS** (SvelteKit integration).
- Types: `svelte-check` in `pnpm check`; `tsc` doesn't see `.svelte` files.
- Lint/format: Biome; its Svelte support covers script blocks only, markup is unchecked.
- Tests: Vitest + `@testing-library/svelte`; Playwright for E2E and screenshots.
- Everything else (validation, tooling, delivery): [core.md](core.md).
