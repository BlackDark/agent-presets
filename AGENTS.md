# agent-presets

Reference material agents read by URL: stack defaults, repo templates, CI snippets, skills. Nothing here gets installed.

## Writing

- Files read by agents (`stack/`, `templates/`, `snippets/` comments, `skills/`): follow [skills/write-for-agents/SKILL.md](skills/write-for-agents/SKILL.md). Terse, one fact per place.
- `README.md` is for humans: short, well written, every file listed there exists.
- English only. No references to other people's projects (except the credit in README) or private repos.

## Changes

- Leave touched files cleaner than you found them: stale facts, broken pointers, outdated versions get fixed in their own commit; larger issues go to the user.
- Stack change: edit the one `stack/*.md` that owns it, bump "Reviewed <date>" in `stack/core.md`, mention the reason in the commit.
- New or renamed file: update the map in `README.md` and any pointer to it.
- Snippets and the Renovate template stay valid: the `Lint` workflow is green.
- Conventional Commits. Merge: rebase when commits are logically separate, squash otherwise.
