# Tech stack: core

Defaults for new projects and new parts. Take the first choice. Anything else (a listed alternative included): propose it to the user with the reason before building on it, then record it in the project's `AGENTS.md`. Existing code stays on its stack unless the user asks to migrate.

Reviewed 2026-11-09. **(watch)** = likely to change, see [watch.md](watch.md).

Branches, read only when the task touches them:
- Types shared between Go/Rust and TS, OpenAPI, Tauri bindings → [contracts.md](contracts.md)
- UI inside firmware or a router image, or with a hard size budget → [embedded-ui.md](embedded-ui.md)
- Svelte project (chosen or existing) → [svelte.md](svelte.md)
- Desktop app (Tauri) → [desktop.md](desktop.md)
- Browser extension → [browser-extension.md](browser-extension.md)
- VS Code extension → [vscode-extension.md](vscode-extension.md)
- User asks to check the watch list, a gate, or to align a project → [watch.md](watch.md)

## Rules

- One verify entry point per repo: `mise run check`. It wraps the language checks:
  - TS: `pnpm check` = Biome, `tsc --noEmit`, Vitest
  - Rust: `cargo fmt --check && cargo clippy --all-targets -- -D warnings && cargo test`
  - Go: `test -z "$(gofmt -l .)" && go vet ./... && go test ./...`
- Green `mise run check` before every commit.
- New dependency outside this stack: ask first, state why in the commit message.
- Generated files (API clients, bindings, compiled i18n): regenerate, never hand-edit; CI fails on diff.
- Validate every boundary at runtime (requests, config, files, env, generated API responses): Zod.
- Every repo has `.github/renovate.json5` from [templates/renovate.template.json5](../templates/renovate.template.json5).
- No secrets in the repo. No telemetry or tracking unless asked.

## Tool installation

- Global installs need the user's permission: `npm i -g`, `cargo install`, `go install`, `apt`/`dnf`/`brew`, `sudo`, `curl | sh`. Ask with what, where, why. `mise` missing → tell the user.
- **mise** with a repo-local `mise.toml` for every runtime and CLI except Rust (`go:`, `npm:`, `cargo:`, `aqua:` backends when no registry entry). Install with `mise install`.
- Pin exact versions, current latest when added (`mise latest <tool>`, `mise use --pin <tool>`). One version per tool: Renovate bumps concrete versions only.
- **Rust: rustup** with committed `rust-toolchain.toml` (pinned channel, `rustfmt`, `clippy`, targets).
- JS tools (Biome, Vitest, Playwright, Drizzle Kit) are `devDependencies`, run via scripts. mise provides runtimes only.
- CI installs from the same files. Personal overrides: `mise.local.toml` (gitignored).
- Versions change through Renovate PRs or an explicit, mentioned change.

Starters: [snippets/mise.toml](../snippets/mise.toml), [snippets/rust-toolchain.toml](../snippets/rust-toolchain.toml).

## Frontend

- Framework: **React**. Alternative: **Svelte 5** when it fits clearly better (small app, size matters, no TanStack Start needed): propose first. Solid only after the Solid gate in [watch.md](watch.md).
- Full-stack/SSR: **TanStack Start**; server functions instead of a separate API when the app owns its backend. Pin the minor.
- Routing **TanStack Router** (file-based). Server state **TanStack Query**. Tables/lists **TanStack Table** + **TanStack Virtual**.
- Forms: controlled inputs; **TanStack Form** once a form grows; Zod schemas.
- Components: **shadcn/ui** copied into the repo, Tailwind + `class-variance-authority`. Primitives: **Base UI** for new projects (`shadcn init -b base`); Radix projects stay Radix. One primitive lib per repo, see `components.json` (Radix `asChild` vs Base UI `render`).
- CSS: **Tailwind CSS 4**, CSS-first config, tokens as CSS variables, light + dark.
- Icons: **lucide-react**.
- Charts: **uPlot** for time series via one wrapper reading theme CSS variables **(watch)**; own SVG for sparklines/small charts; pie/bar/scatter: own SVG or Apache ECharts (record why).
- i18n: **Paraglide JS**, `messages/{locale}.json`, compiled in `postinstall` before lint/typecheck/tests.
- PWA only when used on phone/tablet.

## Backend

- Language: **TypeScript** by default. **Rust** for performance, low latency, system/binary work. **Go** for small static binaries on devices.
- TS runtime: **Bun** (`bun build --compile` for one binary). Node when Bun breaks (test native modules early).
- TS HTTP: TanStack Start server routes; **Hono** for a standalone API.
- Rust: **axum** on **tokio**, `tracing`, `serde`, `reqwest`, `thiserror` (libs), `anyhow` (bins).
- Go: stdlib `net/http` router; a framework only for a reason.
- Validation: **Zod 4**. Size budget (embedded UI, extension popup/content script), any framework → **Zod Mini** (`zod/mini`): same schemas, types, errors. Budget under ~30 KB, or no Zod anywhere in the project → **Valibot**. Form schema min+gzip (2026-10): Zod 26 KB, Zod Mini 6 KB, Valibot 1.5 KB.
- DB: **SQLite** for one server, **PostgreSQL** when shared.
- SQL: **Drizzle** (pin 0.45 while 1.0 is RC **(watch)**), **sqlx** for Rust. Migrations checked in and embedded in the binary.
- Auth: single admin → own login (argon2id, session cookie, CSRF) + **SimpleWebAuthn** passkeys. Multi-user → **Better Auth**, kept current.

## Tooling

- Format/lint TS/JS: **Biome** (replaces ESLint + Prettier). Need full type-aware linting → Oxlint type-aware, record why.
- Types: TS **strict**, `tsc --noEmit` in CI.
- Package manager: **pnpm** (workspaces); **bun** where Bun is the runtime. Lockfile committed, frozen in CI.
- Build: **Vite** (Rolldown); `esbuild` for small scripts.
- Tests: **Vitest** (Browser Mode for components needing a real DOM), `cargo test`, `go test`.
- E2E + screenshots: **Playwright**, pinned, in the official image.

## Delivery

- CI: one `ci.yml` per repo from [snippets/ci.yml](../snippets/ci.yml): draft PRs skip, outdated PR runs cancel, PRs run only the areas they touch (`mise run check` for code, actionlint/zizmor/Renovate validation for workflows and config). The ruleset requires one check, `check`. GitHub matches required checks by name across all workflows, so do not add a second workflow with its own gate: the fastest one would satisfy the ruleset before CI finished. Add a job to `ci.yml` instead. Fast CI is a requirement: cache tools (mise-action), deps (pnpm store) and builds (rust-cache, registry cache for images); Actions caches save on pushes to main only, PRs and tag releases restore; the image registry cache is written by releases. Workflows pass [zizmor](https://docs.zizmor.sh): `persist-credentials: false` on checkout, actions hash-pinned (Renovate bumps the pins), least-privilege `permissions`.
- Server apps: **OCI image** on GHCR (amd64 + arm64, native runners) from [snippets/docker-release.yml](../snippets/docker-release.yml); run with **Podman Quadlets** or Compose. Host-managing apps: one binary + systemd units.
- Releases: [snippets/release-please.yml](../snippets/release-please.yml) opens a release PR from Conventional Commits (version file + CHANGELOG). Merging it tags `vX.Y.Z` and creates the GitHub release, then calls `release.yml` with that tag: a tag pushed with `GITHUB_TOKEN` starts no workflows. Server images: [snippets/docker-release.yml](../snippets/docker-release.yml), called the same way (drop its tag trigger; do not create the release again).
- Docs site: **Zensical** on GitHub Pages **(watch)**.
- Repo texts: [templates/AGENTS.template.md](../templates/AGENTS.template.md), [templates/README.template.md](../templates/README.template.md), [templates/renovate.template.json5](../templates/renovate.template.json5).
- Sponsoring: a new repo copies [templates/FUNDING.yml](../templates/FUNDING.yml) to `.github/FUNDING.yml`. An existing repo without that file: ask before adding.
