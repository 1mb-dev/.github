# .github

Shared workflows and org-level defaults for [1mb-dev](https://github.com/1mb-dev).

## Reusable Workflows

### `ci-go.yml` — Go CI

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
| `go-versions` | `["1.24", "1.25"]` | Go versions to test against |
| `lint-go-version` | `1.25` | Go version for linting |
| `golangci-lint-version` | `v2.7.1` | golangci-lint version |
| `coverage-threshold` | `0` | Min coverage % (0 = skip) |
| `run-security` | `true` | Run govulncheck |
| `build-binary` | `""` | Main package path (empty = skip) |

### `ci-node.yml` — Node/TS CI

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
| `lint-node-version` | `22` | Node version for linting |
| `run-audit` | `true` | Run npm audit |
| `run-build` | `true` | Run npm run build |

### `dependabot-auto-merge.yml` — Auto-merge Dependabot PRs

Auto-merges patch and minor dependency updates when CI passes.

```yaml
name: Dependabot auto-merge
on: pull_request
jobs:
  auto-merge:
    uses: 1mb-dev/.github/.github/workflows/dependabot-auto-merge.yml@main
```

**Prerequisite:** Enable "Allow auto-merge" in repo settings.

## Org Profile

`profile/README.md` is displayed on the [1mb-dev org page](https://github.com/1mb-dev).
