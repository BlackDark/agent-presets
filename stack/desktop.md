# Tech stack: desktop apps

- Framework: **Tauri 2** + the frontend stack from [core.md](core.md). No window needed → plain Rust binary.
- Linux build needs WebKitGTK (system package: ask before installing).
- Structure: desktop crate outside the main Cargo workspace, own CI job (keeps WebKitGTK out of other builds).
- Commands: thin `#[tauri::command]` at the edge, logic in pure Rust functions with unit tests. Bindings: tauri-specta ([contracts.md](contracts.md)).
- Security: minimal capabilities per window, strict CSP, no remote content in the window.
- Plugins: official ones as needed (`updater`, `single-instance`, `window-state`, `store`, `dialog`, `notification`, `autostart`, `deep-link`, `log`). Helper programs as sidecars (`bundle.externalBin`).
- Packages: Linux AppImage (+ `.deb`/`.rpm`/Flatpak if expected), Windows NSIS, macOS `.dmg`. macOS needs Apple Developer account + notarization; Windows needs a signing cert, otherwise first-start warnings.
- Updates: `tauri-plugin-updater` with its own signing key, `latest.json` in the GitHub release.
- Release: **tauri-action**; one version in `Cargo.toml`, `tauri.conf.json`, `package.json`, bumped together.
- Tests: Playwright against the Vite dev server with `mockIPC` (also screenshots); Vitest for stores/logic; Rust tests for the core. Real window needed → WebdriverIO Tauri service (`tauri-driver`: Linux + Windows only).
