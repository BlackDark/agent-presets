# Tech stack: embedded UI

A web UI that ships inside firmware or a router image (ESP32, OpenWrt) and is served by the device.

## Ask first

Device UIs differ by orders of magnitude (an ESP32 with 1 MB flash vs a router with 60 MB overlay). Before choosing a framework, ask the user and record the answers in `AGENTS.md`:

1. Free space for the UI and typical link (phone over hotel WiFi, LAN only).
2. Strict mode or not:
   - **Strict**: a size budget from [size-budget.md](size-budget.md); every dependency weighed against it.
   - **Relaxed** (default when the device has megabytes free): the normal stack for the chosen framework; no budget; only the device rules below apply.

## Framework, in order

1. **Svelte 5** static SPA: [svelte.md](svelte.md), including its libraries (TanStack Query/Form/Table, shadcn-svelte).
2. **React**: when the UI shares code with a React app, or the user prefers it and the device has the room; [core.md](core.md) frontend with TanStack Router in hash mode.
3. **Solid**, only after the Solid gate in [watch.md](watch.md).
4. **Vanilla TS**, only for strict budgets under ~15 KB gzip or pages inside LuCI (use LuCI's JS view API as a `luci-app-*`).

Choosing anything but the first: propose to the user with the reason, record it in `AGENTS.md`.

## Device rules

- No CDN or external fonts: the device is often offline. System font stack; icons imported per icon, no icon font.
- Ship pre-compressed `*.gz`; the device server sends `Content-Encoding: gzip`. Hash routing or one page: the device server has no history fallback.
- Flash file systems with path limits (ESP32 SPIFFS/LittleFS): flat file names, plain Vite with `build.assetsDir: ''`.
- The API lives in the firmware (C/C++, Go, Rust); define it once ([contracts.md](contracts.md)). Business rules stay on the device; UI concerns (caching, polling, forms, tables) use the frontend libraries.
- Charts: chart rule in [core.md](core.md).
- Dev: Vite dev server proxies `/api` to the device or to a mock.
