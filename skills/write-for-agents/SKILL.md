---
name: write-for-agents
description: Rules for text that agents read: AGENTS.md, CLAUDE.md, skills, prompts, docs linked from them. Use when creating, editing or reviewing such a file.
---

# Write for agents

Agent text is loaded into a context window and acted on. Optimise for predictable behaviour per token. Human text (README, PR bodies) optimises for a reader instead: keep it short but well written.

## Budget

- Always-loaded text (AGENTS.md, skill `description`) costs tokens and attention on every turn. Every line must change behaviour.
- Material only some tasks need goes into a separate file behind a pointer. The pointer states what it is and when to read it: "Desktop app (Tauri) → stack/desktop.md".
- Inline what every path needs; disclose what only some paths need.

## Content

- Cache only what the environment can't tell: conventions, reasons, gotchas, the one verify command. Scripts, deps, layout and versions live in `package.json`, `mise.toml` and the tree; restating them goes stale.
- One fact, one place. Link instead of repeating.
- Cut no-ops: instructions the model already follows by default ("write clean code", "be careful").
- Say what to do. A ban names the unwanted thing and makes it likelier; use one only as a hard guardrail, paired with the wanted action.
- Every step ends on a checkable done-state: "`mise run check` green", not "verify it works".
- Prefer an established term (*tracer bullet*, *red/green*, *edge*) over a sentence describing it.

## Form

- Imperative, terse, fragments fine. No preamble, no motivation unless the reason changes behaviour.
- Bullets and `key: value` over prose and wide tables; code blocks for exact commands.
- Group a topic's rule, exception and reason under one heading.
- Date facts that age ("Reviewed 2026-10") and mark volatile ones.

## Skills

- Frontmatter `name` + `description`. The description is the trigger: lead with the subject, list each distinct case once, no synonyms.
- A skill only the user starts: set `disable-model-invocation: true` and a one-line human description; it then costs no context.
- Body over ~150 lines → split branches into sibling files the body points to.

## Done

Every line passes: would removing it change what the agent does? If not, remove it.
