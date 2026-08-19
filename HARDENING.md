<!-- markdownlint-disable -->

# Hardening Report: lerebear--sizeup-action/v0.8.8

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **lerebear--sizeup-action/v0.8.8** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable tag refs instead of pinned 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced tag is moved or the upstream repository is compromised.

Failing references:
- check-dist.yml: actions/checkout@v4, actions/setup-node@v3, actions/upload-artifact@v3
- ci.yml: actions/checkout@v4, actions/setup-node@v3
- sizeup.yaml: lerebear/sizeup-action@v0.8.2

All should be pinned to full 40-character SHA digests, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/check-dist.yml:33`
- `.github/workflows/check-dist.yml:38`
- `.github/workflows/check-dist.yml:60`
- `.github/workflows/ci.yml:16`
- `.github/workflows/ci.yml:22`
- `.github/workflows/sizeup.yaml:16`

### missing-permissions (severity: medium)

The workflow file ci.yml has no top-level `permissions:` block and its only job (`test-typescript`) also has no job-level `permissions:` block. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (e.g. write access to contents). A minimal permissions block such as `permissions: read-all` or specific scopes (e.g. `contents: read`) should be added.

Locations:

- `.github/workflows/ci.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references by pinning to full 40-character commit SHAs:
- check-dist.yml: actions/checkout@v4 → @34e114876b0b11c390a56381ad16ebd13914f8d5, actions/setup-node@v3 → @3235b876344d2a9aa001b8d1453c930bba69e610, actions/upload-artifact@v3 → @ff15f0306b3f739f7b6fd43fb5d26cd321bd4de5
- ci.yml: actions/checkout@v4 → @34e114876b0b11c390a56381ad16ebd13914f8d5, actions/setup-node@v3 → @3235b876344d2a9aa001b8d1453c930bba69e610
- sizeup.yaml: lerebear/sizeup-action@v0.8.2 → @ed1dfed124077e91d3736a1acfddded03aff4c91

Added top-level `permissions: contents: read` block to ci.yml to enforce least-privilege token access. The check-dist.yml already had a job-level permissions block and sizeup.yaml already had a top-level permissions block, so no changes were needed there.

