# CI: path filters, tools, merge policy

## Path filters

- The required workflow runs on every PR; `check` aggregates area results (a skipped workflow reports no check).
- `git diff --no-renames -z`: renames count both sides, filenames stay whole.
- Map every input of an area: config, lockfiles, build scripts, Dockerfiles, `.dockerignore`, the workflow itself.
- Run `actionlint` without paths (finds `.yml` and `.yaml`); pass snippets outside `.github/workflows` separately.

## Tool integrity

- With mise: pin actionlint in `mise.toml`, install via the SHA-pinned mise action ([snippet](../snippets/ci.yml)). Mise config changes select `lint`. Set the action's `version` input if the mise binary must be pinned.
- Without mise: pin `ACTIONLINT_VERSION` in `env:` with a `# renovate:` annotation; download the release archive, verify it, then extract (actionlint >= 1.7.11, [upstream](https://github.com/rhysd/actionlint/blob/main/docs/install.md#prebuilt-binaries)):

```yaml
- env:
    GH_TOKEN: ${{ github.token }}
  run: |
    archive="actionlint_${ACTIONLINT_VERSION}_linux_amd64.tar.gz"
    gh release download "v${ACTIONLINT_VERSION}" --repo rhysd/actionlint --pattern "$archive"
    gh attestation verify "$archive" --repo rhysd/actionlint
    tar -xzf "$archive" actionlint
    ./actionlint -color
```

- Attestation proves provenance, not review; keep tool bumps as reviewable PRs.
- Committed generated output: regenerate, `git add --intent-to-add`, then `git diff --exit-code HEAD -- <output>` (catches deletions and new files).
- Release jobs: test outputs on push, tag and dispatch; PR-only runs never exercise them.
- Draft-to-ready gates: require checks from after the ready transition; a queued suite without runs is no evidence. A no-check fallback delay counts from the later of ready transition and newest suite.

## Merge queues

- Worth it for busy protected branches; costs CI runs and latency. Low volume: up-to-date branch checks instead.
- Needs repo admin and an org repo (private: Enterprise Cloud) ([docs](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/configuring-pull-request-merges/managing-a-merge-queue)).
- Before enabling: add `merge_group` to every required workflow, run all areas on merge-group commits, test a queued merge and a failing check. Merge-group jobs stay read-only, no cache writes.
