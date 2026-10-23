# Tech stack: embedded UI

A web UI that ships inside firmware or a router image (ESP32, OpenWrt), or any UI with a hard size budget.

## Framework, in order

1. **Svelte 5** + plain Vite, static SPA: [svelte.md](svelte.md).
2. **Solid**, only after the Solid gate in [watch.md](watch.md).
3. **React**, only when the UI shares code with a React app; propose it with the size cost first.
4. **Vanilla TS**, only for budgets under ~15 KB gzip or pages inside LuCI (use LuCI's JS view API as a `luci-app-*`).

Choosing anything but the first: propose to the user with the reason, record it in `AGENTS.md`.

## Rules

- Size budget in `mise run check`: `size-limit` with `@size-limit/file`, config `.size-limit.json`. Default 50 KB gzip for the whole UI; set the real one in `AGENTS.md`.
  ```json
  [{ "path": "dist/**/*.{html,js,css,svg}", "limit": "50 KB", "gzip": true }]
  ```
- Validation: size-budget rule in [core.md](core.md) (Zod Mini, Valibot for tiny budgets).
- Only the shadcn-svelte components the UI uses; charts as own SVG; no icon font, only imported icons.
- No CDN or external fonts: the device is often offline. System font stack.
- Ship pre-compressed `*.gz`; the device server sends `Content-Encoding: gzip`. Flat file names, no deep routes: hash routing or one page.
- The API lives in the firmware (C/C++, Go, Rust); define it once ([contracts.md](contracts.md)) and keep the UI a thin client.
- Login on a LAN IP: no passkeys, even over HTTPS: the RP ID must be a domain. See [auth.md](auth.md).
- Dev: Vite dev server proxies `/api` to the device or to a mock.
