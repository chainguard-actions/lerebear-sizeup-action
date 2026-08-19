<!-- markdownlint-disable -->

# Hardening Report: lerebear--sizeup-action/v0.8.11

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **lerebear--sizeup-action/v0.8.11** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable tags instead of pinned full-length SHA commit hashes. This exposes the workflow to supply-chain attacks where a tag could be moved to point to malicious code.

- check-dist.yml: `actions/checkout@v4`, `actions/setup-node@v3`, `actions/upload-artifact@v4`
- ci.yml: `actions/checkout@v4`, `actions/setup-node@v3`
- sizeup.yaml: `lerebear/sizeup-action@v0.8.2`

All of these should be replaced with their full 40-character SHA digest, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/check-dist.yml:30`
- `.github/workflows/check-dist.yml:34`
- `.github/workflows/check-dist.yml:57`
- `.github/workflows/ci.yml:17`
- `.github/workflows/ci.yml:22`
- `.github/workflows/sizeup.yaml:14`

### missing-permissions (severity: medium)

The workflow file `ci.yml` has no top-level `permissions:` key and its only job (`test-typescript`) also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (e.g. `write` access to contents). A minimal permissions block such as `permissions: read-all` or specific scopes (e.g. `contents: read`) should be added.

Locations:

- `.github/workflows/ci.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all unpinned action references to full SHA digests: actions/checkout@v4 → 11d5960a326750d5838078e36cf38b85af677262, actions/setup-node@v3 → 3235b876344d2a9aa001b8d1453c930bba69e610, actions/upload-artifact@v4 → ea165f8d65b6e75b540449e92b4886f43607fa02, lerebear/sizeup-action@v0.8.2 → ed1dfed124077e91d3736a1acfddded03aff4c91. Original tags preserved as inline comments. Added top-level `permissions: contents: read` block to ci.yml to satisfy the missing-permissions finding.

