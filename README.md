<div align="center">

# agent-presets

**Starting points for coding agents: a curated tech stack, repo templates, CI snippets and writing rules, all in one place to link to.**<br>
Point the agent here instead of re-explaining preferences in every repo.

[![CI](https://github.com/BlackDark/agent-presets/actions/workflows/ci.yml/badge.svg)](https://github.com/BlackDark/agent-presets/actions/workflows/ci.yml)
[![License](https://img.shields.io/github/license/BlackDark/agent-presets)](LICENSE)

</div>

## Why

Agents need the same preferences in every repo, and re-pasting them costs time and tokens. Files that agents read are written terse; this README is the human entry point.

- **One link**: the agent reads the stack, template or snippet it needs, nothing else.
- **Cheap context**: agents load only the stack branch their task needs.
- **Pinned defaults**: one verify command and pinned tools across repos.

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
| [`stack/embedded-ui.md`](stack/embedded-ui.md) | UIs inside firmware or router images (ESP32, OpenWrt), size budgets |
| [`stack/svelte.md`](stack/svelte.md) | Svelte projects: the stack mapped to Svelte |
| [`stack/desktop.md`](stack/desktop.md) | Tauri desktop apps |
| [`stack/browser-extension.md`](stack/browser-extension.md) | Browser extensions (MV3) |
| [`stack/vscode-extension.md`](stack/vscode-extension.md) | VS Code extensions |
| [`stack/watch.md`](stack/watch.md) | Gates, watch list, alignment of older projects |
| [`templates/AGENTS.template.md`](templates/AGENTS.template.md) | `AGENTS.md` for a repo (with `CLAUDE.md` → `@AGENTS.md`) |
| [`templates/README.template.md`](templates/README.template.md) | README that says what it is, installs in one command, and gets found |
| [`templates/code-review.prompt.md`](templates/code-review.prompt.md) | Prompt for a thorough, scenario-backed code review |
| [`snippets/ci.yml`](snippets/ci.yml) | The CI for every repo: draft PRs skip, outdated runs cancel, PRs run only the areas they touch (`mise run check`, workflow and Renovate lint), caches saved on main and restored on PRs, one `check` gate for rulesets |
| [`snippets/release-please.yml`](snippets/release-please.yml), [`release-please-config.json`](snippets/release-please-config.json), [`release-please-manifest.json`](snippets/release-please-manifest.json) | Conventional Commits → release PR → tag + GitHub release; calls `release.yml` with the tag |
| [`snippets/docker-release.yml`](snippets/docker-release.yml) | Tag → multi-arch image on GHCR (native arm64) + GitHub release |
| [`templates/renovate.template.json5`](templates/renovate.template.json5) | `.github/renovate.json5` for every repo: extends `renovate-base`; adds Node group, majors by hand |
| [`templates/FUNDING.yml`](templates/FUNDING.yml) | `.github/FUNDING.yml`: Ko-fi and GitHub Sponsors. New repo: add it. Existing repo: ask first |
| [`snippets/mise.toml`](snippets/mise.toml), [`rust-toolchain.toml`](snippets/rust-toolchain.toml) | Pinned tools |
| [`docs/agents/`](docs/agents/) | This repo's agent-skills config: issue tracker, triage labels, domain docs |
| [`skills/write-for-agents`](skills/write-for-agents/SKILL.md) | How to write AGENTS.md, skills and prompts that stay small and predictable |

## Principles

- **One verify command**: every repo answers to `mise run check`, for humans, agents and CI alike.
- **Pinned and repo-local**: tools come from `mise.toml`/`rust-toolchain.toml`, never global installs; Renovate bumps them.
- **Defaults, not dogma**: deviations are fine when the reason is written into the project's `AGENTS.md`.
- **Cheap context**: agents load only the stack branch their task needs.

---

<div align="center"><sub>

[Report a bug](https://github.com/BlackDark/agent-presets/issues/new) · License: [MIT](LICENSE)<br>
Inspired by [firsttris/workflows](https://github.com/firsttris/workflows).

</sub></div>
