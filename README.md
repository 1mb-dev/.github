# .github

Shared workflows and org-level defaults for [1mb-dev](https://github.com/1mb-dev).

## Reusable Workflows

### `ci-go.yml` -- Go CI

Lint, test, security scan, and build for Go projects.

```yaml
# In your repo's .github/workflows/ci.yml
name: CI
on: [push, pull_request]
jobs:
  ci:
    uses: 1mb-dev/.github/.github/workflows/ci-go.yml@main
```

**Inputs (all optional):**

| Input | Default | Description |
|-------|---------|-------------|
| `go-versions` | `["1.25", "1.26"]` | Go versions to test against |
| `lint-go-version` | `"1.26"` | Go version for linting |
| `golangci-lint-version` | `v2.9.0` | golangci-lint version |
| `coverage-threshold` | `0` | Min coverage % (0 = skip) |
| `run-security` | `true` | Run govulncheck |
| `run-secret-scan` | `true` | Run gitleaks via `secret-scan.yml` |
| `build-binary` | `""` | Main package path (empty = skip) |

### `ci-node.yml` -- Node/TS CI

Lint, test, build, and security audit for Node.js/TypeScript projects.

```yaml
name: CI
on: [push, pull_request]
jobs:
  ci:
    uses: 1mb-dev/.github/.github/workflows/ci-node.yml@main
```

**Inputs (all optional):**

| Input | Default | Description |
|-------|---------|-------------|
| `node-versions` | `["20", "22"]` | Node versions to test against |
| `lint-node-version` | `"22"` | Node version for linting |
| `run-audit` | `true` | Run npm audit |
| `run-secret-scan` | `true` | Run gitleaks via `secret-scan.yml` |
| `run-build` | `true` | Run npm run build |

### `secret-scan.yml` -- Gitleaks secret scan

Scans the repository for committed secrets using gitleaks. Called transitively by `ci-go.yml` and `ci-node.yml` when `run-secret-scan` is `true`; can also be invoked directly for repos that do not use the language CIs.

```yaml
name: Secret scan
on: [push, pull_request]
jobs:
  scan:
    uses: 1mb-dev/.github/.github/workflows/secret-scan.yml@main
```

**Inputs (all optional):**

| Input | Default | Description |
|-------|---------|-------------|
| `gitleaks-version` | `8.21.2` | Gitleaks binary version |

The job fails on any detected secret (`--exit-code 1`). Findings are redacted in logs.

### `dependabot-auto-merge.yml` -- Auto-merge Dependabot PRs

Auto-merges patch and minor dependency updates when CI passes.

```yaml
name: Dependabot auto-merge
on: pull_request
jobs:
  auto-merge:
    uses: 1mb-dev/.github/.github/workflows/dependabot-auto-merge.yml@main
```

**Prerequisite:** Enable "Allow auto-merge" in repo settings.

### `release.yml` -- Tag-driven release

Creates a GitHub release when a version tag is pushed. Title resolution falls through three sources in order: annotated tag message subject, next unused name from `.github/release-names.txt` (per-repo theme), or the tag itself.

```yaml
# In your repo's .github/workflows/release.yml
name: Release
on:
  push:
    tags: ['v*']
jobs:
  release:
    uses: 1mb-dev/.github/.github/workflows/release.yml@main
```

**Inputs (all optional):**

| Input | Default | Description |
|-------|---------|-------------|
| `draft` | `true` | Create as draft (`false` to publish immediately) |

Adding a per-repo `.github/release-names.txt` (one name per line) enables themed release titles assigned in file order.

## Pinning Policy

All shared workflows are consumed via `@main`. This is intentional: a single maintainer plus a small consumer set means fix-once-apply-everywhere is the whole point. Every commit on `.github/main` propagates to all consumers on their next workflow run.

Trade-off: there is no version-pin shock absorber. Breaking changes hit all repos at once. Mitigations: (a) keep shared workflows minimal and stable, (b) consult `CHANGELOG.md` before merging non-trivial changes, (c) revisit pinning if consumer count grows past ~30 or shared workflows start breaking on bumps.

## Org Profile

`profile/README.md` is displayed on the [1mb-dev org page](https://github.com/1mb-dev).
