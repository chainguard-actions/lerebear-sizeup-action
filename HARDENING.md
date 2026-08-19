<!-- markdownlint-disable -->

# Hardening Report: lerebear--sizeup-action/v0.8.9

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **lerebear--sizeup-action/v0.8.9** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable version tags instead of pinned 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the tag is moved to a different (potentially malicious) commit.

- .github/workflows/check-dist.yml: `actions/checkout@v4`, `actions/setup-node@v3`, `actions/upload-artifact@v3`
- .github/workflows/ci.yml: `actions/checkout@v4`, `actions/setup-node@v3`
- .github/workflows/sizeup.yaml: `lerebear/sizeup-action@v0.8.2`

All should be replaced with their full 40-character SHA digest, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/check-dist.yml:30`
- `.github/workflows/check-dist.yml:34`
- `.github/workflows/check-dist.yml:55`
- `.github/workflows/ci.yml:16`
- `.github/workflows/ci.yml:21`
- `.github/workflows/sizeup.yaml:14`

### missing-permissions (severity: medium)

The workflow file `.github/workflows/ci.yml` has no top-level `permissions:` block and none of its jobs define a `permissions:` block either. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be broader than necessary (e.g., write access to contents). A minimal `permissions:` block such as `permissions: read-all` or specific scopes like `contents: read` should be added.

Locations:

- `.github/workflows/ci.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 6 unpinned action references across 3 workflow files by resolving each to its full 40-character commit SHA (preserving the original tag as a comment): actions/checkout@v4 → 34e114876b0b11c390a56381ad16ebd13914f8d5, actions/setup-node@v3 → 3235b876344d2a9aa001b8d1453c930bba69e610, actions/upload-artifact@v3 → ff15f0306b3f739f7b6fd43fb5d26cd321bd4de5, lerebear/sizeup-action@v0.8.2 → ed1dfed124077e91d3736a1acfddded03aff4c91. Added a top-level `permissions: contents: read` block to ci.yml to address the missing-permissions finding. check-dist.yml already had a job-level permissions block and was not modified for permissions. A file corruption during editing of check-dist.yml was corrected by rewriting the file.

