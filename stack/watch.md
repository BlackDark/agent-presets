# Tech stack: gates, watch list, alignment

Report findings to the user; the stack changes only on their decision.

## Solid gate

New projects (general and embedded UI) use Solid only when every box is checked. Existing Solid 1.x code stays. Checked 2026-11-27.

- [x] `solid-js@2` stable (2.0.0, 2026-11-17)
- [ ] Headless primitives stable on Solid 2: Kobalte 2 is alpha; UI SDK (`@ui-sdk/solid`) is beta
- [ ] A shadcn-style Solid port on Solid 2: Solid UI v2 targets Solid 2 on UI SDK, marked beta by its README
- [ ] `@solidjs/router` 2 stable (2.0.1); Solid Testing Library 1.0 is RC; `lucide-solid` still peers Solid 1 (Solid UI inlines SVG icons)
- [ ] TanStack Query, Form, Table on Solid 2: Query 6 is RC; Form and Table stable peer Solid 1
- [ ] Full-stack only: TanStack Router/Start 2 (Solid 2) is RC
- [ ] Extensions only: `@crxjs/vite-plugin` + `@solidjs/vite-plugin` 3 (Solid 2, renamed from `vite-plugin-solid`) build a working extension

## Watch list

| Item | Check | Report when |
|---|---|---|
| Solid primitives | `npm view @kobalte/core dist-tags`, `npm view @ui-sdk/solid dist-tags` | a non-prerelease `latest` peers `solid-js@^2` |
| Solid UI | [README](https://github.com/stefan-karger/solid-ui) | v2 drops the beta warning |
| Solid libraries | `npm view <pkg> peerDependencies` for `@tanstack/solid-{query,form,table,router}`, `@solidjs/testing-library`, `lucide-solid` | `latest` peers `solid-js@^2` |
| Drizzle 1.0 | `npm view drizzle-orm dist-tags` | `latest` is 1.x (upgrade on a branch; migration layout changes) |
| tauri-specta | `cargo search tauri-specta` | non-RC 2.x |
| Zensical | release notes | parity with Material for MkDocs (maintenance ends 2027-05-05) |
| Oxfmt | `npm view oxfmt dist-tags` | stable 1.x |
| Vite+ | VoidZero release notes | proven as one toolchain for Vite, Vitest, Oxlint, Oxfmt |
| Biome type-aware | release notes | `noFloatingPromises` leaves nursery |
| uPlot | `npm view uplot time` | no release since 2025-03-14 hits 2 years (2027-03), or breaks on a new React/Vite major |
| class-variance-authority | `npm view class-variance-authority dist-tags` | 1.0 stable, or shadcn stops generating it |
| TypeScript 7.1 | `npm view typescript dist-tags` | 7.1 with programmatic API |

## Alignment

Apply only while touching that part anyway:

- ESLint/Prettier → Biome; delete their configs.
- Own CSS → Tailwind on UI rework.
- Large chart library → uPlot or own SVG.
- Own fetch/poll/refresh state → TanStack Query. Own component where the shadcn registry has one → registry component.
- Size limit without a recorded reason → ask for the headroom ([size-budget.md](size-budget.md)).
- npm → pnpm (VS Code extensions may stay).
- Plain TS/esbuild extension → React + Vite + `@crxjs/vite-plugin`.
- VS Code extension built by `tsc` only → Vite bundle.
- Auth.js/NextAuth → Better Auth. Zod 3 → 4.
- Material for MkDocs → Zensical before 2027-05-05.
- `.nvmrc`, `.tool-versions`, Volta, manual installs → `mise.toml`; Rust stays on rustup.
- `ci.yml` without draft skip/concurrency, or a separate `lint.yml` with its own gate → [snippets/ci.yml](../snippets/ci.yml) (one workflow, one `check` gate).
