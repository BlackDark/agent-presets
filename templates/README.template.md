<!--
Template from BlackDark/agent-presets. For the agent filling it in:
- Fill <…> from the repo; drop sections that don't apply; delete every comment.
- Rules:
  - H1 + bold line say what it is in the words people search for; name platforms and what it replaces.
  - Shortest working install command near the top.
  - Why before what: the problem in 2 sentences, then 3–4 benefits (user outcomes, not internals; numbers where real).
  - Screenshot under the badges, current, with descriptive alt text.
  - FAQ headings are questions people type into a search engine.
  - Short: the README sells and gets people started; details go to docs.
  - Repo settings match: description = bold line, website = docs, 5–15 topics.
-->
<div align="center">

# <Name>

**<What it is and for whom, in search words.>**<br>
<What it does for the user. What makes it different.>

[![CI](https://github.com/BlackDark/<repo>/actions/workflows/ci.yml/badge.svg)](https://github.com/BlackDark/<repo>/actions/workflows/ci.yml)
[![Release](https://img.shields.io/github/v/release/BlackDark/<repo>)](https://github.com/BlackDark/<repo>/releases/latest)
[![License](https://img.shields.io/github/license/BlackDark/<repo>)](LICENSE)
<!-- At most one more row: distribution (pulls, installs) or stack (one or two). -->

<img src="docs/screenshot.png" alt="<What the screenshot shows, with keywords>" width="900">

</div>

## Why

<What people use today and where it falls short.>

- **<Benefit>**: <what it means for the user>.
- **<Benefit>**: <…>.
- **<Benefit>**: <…>.

## Install

```bash
<shortest complete command: compose file, Quadlet, store link, binary download>
```

<What happens next, e.g. "Open http://localhost:8080.">. More options: [docs](<url>).

## Features

<!-- 3–6 groups by user task; bold the key term of each bullet. -->

### <Group>

- **<Feature>**: <one line>.

## FAQ

### <Question people search for?>

<Answer first ("Yes." / "No, …"), then one or two sentences and a link.>

## Development

Requires [mise](https://mise.jdx.dev).

```bash
git clone https://github.com/BlackDark/<repo> && cd <repo>
mise install && <install deps>
<start dev server>
mise run check
```

Issues and PRs are welcome; run `mise run check` first.

---

<div align="center"><sub>

[Report a bug](https://github.com/BlackDark/<repo>/issues/new) · License: [<license>](LICENSE)<br>
<Name> is not affiliated with <third-party names used>.

</sub></div>
