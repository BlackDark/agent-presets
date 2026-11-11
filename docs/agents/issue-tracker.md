# Issue tracker: GitHub

Issues and specs live as GitHub issues in `BlackDark/agent-presets`. Use `gh` for all operations.

## Conventions

- **Create**: `gh issue create --title "..." --body "..."`. Heredoc for multi-line bodies.
- **Read**: `gh issue view <n> --json number,title,body,labels,comments`.
- **List**: `gh issue list --state open --json number,title,body,labels,comments`, add `--label` filters as needed.
- **Sub-issue**: `gh issue edit <parent> --add-sub-issue <child>` (`gh` 2.94+). Without sub-issues, put `Part of #<parent>` atop the child body.
- **Blocking**: `gh issue create --blocked-by <n>,<n>` or `gh issue edit <child> --add-blocked-by <n>` (`gh` 2.94+). Fallback: `Blocked by: #<n>` line atop child body. Unblocked when all blockers closed.
- **Count open blockers**: `gh api repos/BlackDark/agent-presets/issues/<n> --jq .issue_dependencies_summary.blocked_by` (0 = startable).
- **Comment**: `gh issue comment <n> --body "..."`.
- **Labels**: `gh issue edit <n> --add-label "..."` / `--remove-label "..."`.
- **Close**: comment with reason, then `gh issue close <n>`.

## Pull requests as a triage surface

**PRs as a request surface: no.** Set to `yes` to route external PRs through triage (`/triage` reads this flag).

## When a skill says "publish to the issue tracker"

Create a GitHub issue.

## When a skill says "fetch the relevant ticket"

Run **Read** above.
