<!--
Template from BlackDark/agent-presets. For the agent filling it in:
- Fill <…> from the repo; delete sections that don't apply and this comment.
- Loaded every session: each line must change behaviour. Cache only what the repo can't tell
  (conventions, reasons, gotchas); leave scripts, deps and layout to package.json, mise.toml, the tree.
- Also create CLAUDE.md containing only the line `@AGENTS.md`.
-->
# <Name>

<One line: what it is and who uses it.>

## Verify

`mise run check` green before every commit. <Extra checks: `mise run e2e` after UI changes, …>

## Stack

Follows https://github.com/BlackDark/agent-presets/blob/main/stack/core.md.
Deviations: <choice: reason>.

## Map

<Only what the tree doesn't explain: where the entry points are, which module owns what, generated dirs.>

## Every change

Same PR, only what the change touches:
- tests for the changed behaviour (<unit, E2E>)
- docs in `<docs/>` that describe it
- README when a user-visible feature changes
- <screenshots after visual changes: `<command or workflow>`>

## Code style

- Data in, data out: pure functions, side effects (I/O, time, randomness) at the edges and passed in.
- Expected failures are values (`Result`/error unions in TS, `Result` in Rust, `error` in Go); throw/panic for bugs only.
- Composition over inheritance: no class hierarchies. Rust traits, Go methods, framework-required classes are fine.
- Small units: split a file past ~400 lines, a function past ~60.
- Names explain what, comments explain why.
- Refactors keep behaviour and land in their own commit.

## Git

- Conventional Commits. One topic per PR.
- PR already merged → branch anew from `origin/main`, open a new PR.
- <Merge method: squash. Force-push: `--force-with-lease` on own branches only.>

## Boundaries

- `sudo`, system packages, global installs: the user runs them; ask with what and why.
- Running user processes stay up; test on other ports.
- Language: code, comments, docs, commits in English; UI in <languages>.

## Gotchas

<Non-obvious traps a fresh session hits: devices, accounts, test data, flaky services, env vars.>
