<!-- markdownlint-disable -->

# Hardening Report: android-actions--setup-android/v3.2.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **android-actions--setup-android/v3.2.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references in the workflow files use mutable tag-based refs instead of full 40-character SHA commit digests. This exposes the action to supply-chain attacks where a tag could be silently moved to point to malicious code.

Failing references in build-test.yml:
- actions/checkout@v4 (multiple occurrences)
- actions/setup-node@v4 (multiple occurrences)
- actions/setup-java@v4

Failing references in codeql-analysis.yml:
- actions/checkout@v4
- github/codeql-action/init@v2
- github/codeql-action/analyze@v2

Failing references in run.yml:
- actions/checkout@v4 (multiple occurrences)
- actions/setup-java@v4

All should be pinned to full SHA digests, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/build-test.yml:16`
- `.github/workflows/codeql-analysis.yml:30`
- `.github/workflows/run.yml:30`

### missing-permissions (severity: medium)

The workflow files `build-test.yml` and `run.yml` have no top-level `permissions:` key and no job-level `permissions:` key on any of their jobs. Without explicit permissions, GitHub Actions defaults to broad repository permissions (typically `contents: write` for push-triggered workflows), violating the principle of least privilege. Each workflow or each job should declare the minimal permissions required.

Locations:

- `.github/workflows/build-test.yml:1`
- `.github/workflows/run.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all three workflow files:

1. **unpinned-uses** — Pinned all `uses:` references to full 40-character SHA digests with original tags preserved as comments:
   - `actions/checkout@v4` → `actions/checkout@11d5960a326750d5838078e36cf38b85af677262 # v4` (all occurrences in build-test.yml, codeql-analysis.yml, run.yml)
   - `actions/setup-node@v4` → `actions/setup-node@49933ea5288caeca8642d1e84afbd3f7d6820020 # v4` (all occurrences in build-test.yml)
   - `actions/setup-java@v4` → `actions/setup-java@c1e323688fd81a25caa38c78aa6df2d33d3e20d9 # v4` (build-test.yml and run.yml)
   - `github/codeql-action/init@v2` → `github/codeql-action/init@b8d3b6e8af63cde30bdc382c0bc28114f4346c88 # v2` (codeql-analysis.yml)
   - `github/codeql-action/analyze@v2` → `github/codeql-action/analyze@b8d3b6e8af63cde30bdc382c0bc28114f4346c88 # v2` (codeql-analysis.yml)

2. **missing-permissions** — Added top-level `permissions: contents: read` to both `build-test.yml` and `run.yml`. The `codeql-analysis.yml` already had appropriate job-level permissions (`actions: read`, `contents: read`, `security-events: write`) and did not need changes.

