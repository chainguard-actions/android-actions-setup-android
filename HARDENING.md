<!-- markdownlint-disable -->

# Hardening Report: android-actions--setup-android/v3.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **android-actions--setup-android/v3.2.0** was hardened automatically. 5 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All uses: references in build-test.yml use mutable tag refs instead of pinned 40-character SHA commit hashes. Failing references: actions/checkout@v4, actions/setup-node@v4, actions/setup-java@v3. These can be silently updated by the upstream maintainer, enabling supply-chain attacks.

Locations:

- `.github/workflows/build-test.yml:16`
- `.github/workflows/build-test.yml:20`
- `.github/workflows/build-test.yml:35`
- `.github/workflows/build-test.yml:39`
- `.github/workflows/build-test.yml:51`
- `.github/workflows/build-test.yml:55`
- `.github/workflows/build-test.yml:72`
- `.github/workflows/build-test.yml:76`
- `.github/workflows/build-test.yml:80`
- `.github/workflows/build-test.yml:91`
- `.github/workflows/build-test.yml:95`

### unpinned-uses (severity: high)

All uses: references in codeql-analysis.yml use mutable tag refs instead of pinned 40-character SHA commit hashes. Failing references: actions/checkout@v4, github/codeql-action/init@v2, github/codeql-action/analyze@v2.

Locations:

- `.github/workflows/codeql-analysis.yml:28`
- `.github/workflows/codeql-analysis.yml:32`
- `.github/workflows/codeql-analysis.yml:37`

### unpinned-uses (severity: high)

All uses: references in run.yml use mutable tag refs instead of pinned 40-character SHA commit hashes. Failing references: actions/checkout@v4 (twice), actions/setup-java@v3.

Locations:

- `.github/workflows/run.yml:26`
- `.github/workflows/run.yml:30`
- `.github/workflows/run.yml:34`

### missing-permissions (severity: medium)

build-test.yml has no top-level permissions: key and none of its jobs (build, format-check, test, runSdkManager, lint) define a job-level permissions: block. Without explicit permissions, the GITHUB_TOKEN is granted its default (potentially broad) permissions.

Locations:

- `.github/workflows/build-test.yml:1`

### missing-permissions (severity: medium)

run.yml has no top-level permissions: key and its only job (build) has no job-level permissions: block. Without explicit permissions, the GITHUB_TOKEN is granted its default (potentially broad) permissions.

Locations:

- `.github/workflows/run.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all three workflow files:

1. build-test.yml:
   - Added top-level `permissions: contents: read` block
   - Pinned actions/checkout@v4 → @34e114876b0b11c390a56381ad16ebd13914f8d5 (all 4 occurrences)
   - Pinned actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020 (all 5 occurrences)
   - Pinned actions/setup-java@v3 → @17f84c3641ba7b8f6deff6309fc4c864478f5d62 (1 occurrence)

2. codeql-analysis.yml:
   - Pinned actions/checkout@v4 → @34e114876b0b11c390a56381ad16ebd13914f8d5
   - Pinned github/codeql-action/init@v2 → @b8d3b6e8af63cde30bdc382c0bc28114f4346c88
   - Pinned github/codeql-action/analyze@v2 → @b8d3b6e8af63cde30bdc382c0bc28114f4346c88
   - Job-level permissions were already present (actions: read, contents: read, security-events: write)

3. run.yml:
   - Added top-level `permissions: contents: read` block
   - Pinned actions/checkout@v4 → @34e114876b0b11c390a56381ad16ebd13914f8d5 (2 occurrences)
   - Pinned actions/setup-java@v3 → @17f84c3641ba7b8f6deff6309fc4c864478f5d62 (1 occurrence)

All SHAs were resolved using lookup_action_sha and preserved with inline tag comments for readability.

