# Tech stack: gates, watch list, alignment

Report findings to the user; the stack changes only on their decision.

## Solid gate

New projects use Solid only when every box is checked. Existing Solid 1.x code stays. 2026-10-18: Solid 2.0 is RC.

- [ ] `solid-js@2` stable
- [ ] Kobalte stable on Solid 2
- [ ] A shadcn-style Solid port on Solid 2
- [ ] `@solidjs/router`, `lucide-solid`, Vitest + Solid Testing Library work on Solid 2
- [ ] Full-stack only: TanStack Router/Start support Solid 2
- [ ] Extensions only: `@crxjs/vite-plugin` + `vite-plugin-solid` build a working extension

## Watch list

| Item | Check | Report when |
|---|---|---|
| Solid 2 | `npm view solid-js dist-tags` | `latest` is 2.x |
| Kobalte | `npm view @kobalte/core dist-tags` | stable on Solid 2 |
| Drizzle 1.0 | `npm view drizzle-orm dist-tags` | `latest` is 1.x (upgrade on a branch; migration layout changes) |
| tauri-specta | `cargo search tauri-specta` | non-RC 2.x |
| Zensical | release notes | parity with Material for MkDocs (maintenance ends 2027-05-05) |
| Oxfmt | `npm view oxfmt dist-tags` | stable 1.x |
| Vite+ | VoidZero release notes | proven as one toolchain for Vite, Vitest, Oxlint, Oxfmt |
| Biome type-aware | release notes | `noFloatingPromises` leaves nursery |
| uPlot | `npm view uplot time` | no release for 2+ years, or breaks on a new React/Vite major |
| TypeScript 7.1 | `npm view typescript dist-tags` | 7.1 with programmatic API |

## Alignment

Apply only while touching that part anyway:

- ESLint/Prettier → Biome; delete their configs.
- Own CSS → Tailwind on UI rework.
- Large chart library → uPlot or own SVG.
- npm → pnpm (VS Code extensions may stay).
- Plain TS/esbuild extension → React + Vite + `@crxjs/vite-plugin`.
- VS Code extension built by `tsc` only → Vite bundle.
- Auth.js/NextAuth → Better Auth. Zod 3 → 4.
- Material for MkDocs → Zensical before 2027-05-05.
- `.nvmrc`, `.tool-versions`, Volta, manual installs → `mise.toml`; Rust stays on rustup.
- `ci.yml` without draft skip/concurrency → [snippets/ci.yml](../snippets/ci.yml) header.
