# agent-presets

**Starting points for coding agents: a curated tech stack, repo templates, CI snippets and writing rules, all in one place to link to.**

Instead of re-explaining preferences in every repo, point the agent here. Files that agents read are written terse to save tokens; this README is the human entry point.

## Use it

Paste into an agent session in the target repo:

```text
Read https://github.com/BlackDark/agent-presets/blob/main/templates/AGENTS.template.md
and create AGENTS.md for this repository.
```

```text
Use the stack in https://github.com/BlackDark/agent-presets/blob/main/stack/core.md
for this new project.
```

```text
Copy snippets/ci.yml from BlackDark/agent-presets and adapt it to this repo.
```

## What's inside

| Path | For |
|---|---|
| [`stack/core.md`](stack/core.md) | Default stack, tool installation rules (mise, rustup), verify command. Links the branches below |
| [`stack/contracts.md`](stack/contracts.md) | Shared types between Go/Rust and TypeScript, OpenAPI |
| [`stack/desktop.md`](stack/desktop.md) | Tauri desktop apps |
| [`stack/browser-extension.md`](stack/browser-extension.md) | Browser extensions (MV3) |
| [`stack/vscode-extension.md`](stack/vscode-extension.md) | VS Code extensions |
| [`stack/watch.md`](stack/watch.md) | Gates, watch list, alignment of older projects |
| [`templates/AGENTS.template.md`](templates/AGENTS.template.md) | `AGENTS.md` for a repo (with `CLAUDE.md` → `@AGENTS.md`) |
| [`templates/README.template.md`](templates/README.template.md) | README that says what it is, installs in one command, and gets found |
| [`templates/code-review.prompt.md`](templates/code-review.prompt.md) | Prompt for a thorough, scenario-backed code review |
| [`snippets/ci.yml`](snippets/ci.yml) | CI header: draft PRs skip, outdated runs cancel, `mise run check` |
| [`snippets/docker-release.yml`](snippets/docker-release.yml) | Tag → multi-arch image on GHCR (native arm64) + GitHub release |
| [`templates/renovate.template.json5`](templates/renovate.template.json5) | `.github/renovate.json5` for every repo: weekly, automerge all but majors, grouped, Go tidy |
| [`snippets/mise.toml`](snippets/mise.toml), [`rust-toolchain.toml`](snippets/rust-toolchain.toml) | Pinned tools |
| [`skills/write-for-agents`](skills/write-for-agents/SKILL.md) | How to write AGENTS.md, skills and prompts that stay small and predictable |

## Principles

- **One verify command**: every repo answers to `mise run check`, for humans, agents and CI alike.
- **Pinned and repo-local**: tools come from `mise.toml`/`rust-toolchain.toml`, never global installs; Renovate bumps them.
- **Defaults, not dogma**: deviations are fine when the reason is written into the project's `AGENTS.md`.
- **Cheap context**: agents load only the stack branch their task needs.

Inspired by [firsttris/workflows](https://github.com/firsttris/workflows). License: [MIT](LICENSE).
