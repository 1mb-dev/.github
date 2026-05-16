# Changelog

Notable changes to shared workflows and org defaults.

## [Unreleased]

### Changed

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
