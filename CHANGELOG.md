# Changelog

Notable changes to shared workflows and org defaults.

## [Unreleased]

### Added

- `ci-go.yml`, `ci-node.yml`: `CI Result` job needing every job, the check rulesets require. `Test Result` needed only `test`, so a PR red on Security still showed it green.

### Changed

- `ci-go.yml`: Go defaults `["1.26", "1.27"]`. Go 1.25 is EOL. Lint stays on Go 1.26 with golangci-lint `v2.9.0` until every consumer is clean under v2.13.2 (the first release with Go 1.27 support is v2.13.0); it must move before Go 1.28 drops 1.26 from the matrix.
- `ci-node.yml`: Node defaults `["22", "24"]`, lint on `24`. Node 20 is EOL.
- `check-latest: true` on every `setup-go` / `setup-node` step, so patches track the latest release instead of the runner cache. Majors still move only by a commit here.

### Security

- `secret-scan.yml`: verify the gitleaks tarball's sha256 before extracting (was `curl | tar`). New `gitleaks-sha256` input, required when the version is overridden.
- `ci-go.yml`, `dependabot-auto-merge.yml`: pin `golangci/golangci-lint-action` (v9.3.0) and `dependabot/fetch-metadata` (v3.1.0) by commit SHA. The auto-merge job holds `contents: write`.
- `dependabot-auto-merge.yml`: gate on the PR author (`github.event.pull_request.user.login`) instead of `github.actor`, which is whoever triggered the run.

## [shared-v1] - 2026-05-16

First marked release of the shared workflows. Captures the stable state after the May 2026 hygiene cycle.

### Changed

- Bump `dependabot/fetch-metadata` from `v2` to `v3` in `dependabot-auto-merge.yml` (Node 24 runtime).
- Bump gitleaks default from `8.21.2` to `8.30.1` in `secret-scan.yml`.
- README: correct stale Go and golangci defaults, document `secret-scan.yml` and `release.yml`, add `run-secret-scan` to Go and Node input tables, add Pinning Policy section.

## 2026-04-27

### Added

- `secret-scan.yml` -- standalone gitleaks workflow. Called transitively by `ci-go.yml` and `ci-node.yml` via the `run-secret-scan` input.
- `release.yml` -- tag-driven GitHub release with three-tier title resolution: annotated tag subject -> `.github/release-names.txt` next-unused name -> tag itself.

## 2026-02-26

### Changed

- `ci-go.yml`: bump `setup-go` to v6, remove codecov integration.
- `ci-go.yml`: bump golangci-lint to `v2.9.0` for Go 1.26 compatibility.

## 2026-02-25

### Changed

- `ci-go.yml`: bump Go defaults to `["1.25", "1.26"]`. Fixes GO-2026-4337.
- `ci-go.yml`: split test coverage from compatibility matrix.

## 2026-02-15

### Added

- Initial reusable CI workflows: `ci-go.yml`, `ci-node.yml`, `dependabot-auto-merge.yml`.
- Org-level `SECURITY.md`.
- Dependabot config for the `github-actions` ecosystem.
- `profile/README.md` for the [1mb-dev org page](https://github.com/1mb-dev).
