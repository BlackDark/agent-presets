# Tech stack: embedded Rust (ESP32)

Firmware in `no_std` Rust on Embassy with a web UI inside the image ([embedded-ui.md](embedded-ui.md)). Reviewed 2026-10-31 on an ESP32 (Xtensa) project; RISC-V chips (C3, C6) need no `esp` toolchain.

## Layout

- Workspace of host crates on stable Rust: machine logic, protocols, API formats, display. No hardware, no clock: time and I/O come in as arguments. Tested on the host (`cargo test`).
- Drivers take hardware through `embedded-hal` traits, so they run against fakes on the host.
- `firmware/` is a separate workspace on the target toolchain. Only wiring lives there (tasks, pins, flash, network); logic belongs in a crate.
- One task owns the outputs (heater, pump, valve, relays). The panic hook switches them off; it is a safety net, not error handling.

## Toolchain

- Xtensa chips: `firmware/rust-toolchain.toml` with `channel = "esp"`, installed by **espup**. Pin espup in `mise.toml` (`"github:esp-rs/espup"`) and the Rust version in the `espup install` line. Renovate cannot bump the `esp` channel: change both together.
- `espup install --targets esp32 --toolchain-version <x.y.z.w> --skip-version-parse --export-file firmware/.esp-env.sh`. Gitignore the export file; recipes source it. `--skip-version-parse` is needed where the GitHub API is rate-limited (CI).
- Entry point is a `justfile` (firmware recipes need the env file); `mise run check` calls `just check`. `check` = fmt, clippy (smallest and largest feature set), host tests, UI checks.

## Flash and size

- Merged image for first install; OTA slots hold the app only. Size guard in `check`: app under ~90 % of one OTA slot.
- USB flash erases `otadata` (`--erase-parts otadata`), or the bootloader keeps starting the slot an earlier OTA update wrote.
- Without a terminal (agents, CI) `espflash flash --monitor` fails after flashing; flash without it, then `espflash monitor --non-interactive` under `timeout`.
- espflash rejects custom data partition subtypes: use `undefined` and find the partition by label.
- Reads need the user's hardware: say what to run, do not guess a serial port.

## Gotchas

- Plain GPIOs 34-39 on the ESP32 have no pulls: switches need external resistors.
- A GPIO shared with UART TX silences the serial log when used.
- Big buffers go on the heap, not in task futures; allocations a request can trigger are fallible.
- A task that can block must not share the control executor.

## CI

- Jobs: host (stable), ui (builds `dist`, uploads it), firmware (downloads `dist`, installs Xtensa toolchain). One `check` job aggregates them ([snippets/ci-paths.yml](../snippets/ci-paths.yml)).
- Cache `~/.rustup/toolchains/esp` and the export file, key = toolchain version.
