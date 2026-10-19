Review <main | this branch against main | PR #n> thoroughly. Report only; change nothing.

Cover every category, write "no findings" where none:
correctness & edge cases (most important) · error handling · security · simplification · performance ·
consistency with the repo · tests · each rule in AGENTS.md separately.

Only real problems: every finding needs a concrete scenario where it causes harm; no scenario, no finding.
Fixes minimal, never riskier than the problem.

Format per finding: 🔴 must / 🟡 should · `file:line` · problem + scenario · fix.
