<!-- markdownlint-disable -->

# Hardening Report: lerebear--sizeup-action/v0.8.10

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **lerebear--sizeup-action/v0.8.10** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All three workflow files reference GitHub Actions using mutable version tags instead of immutable 40-character SHA commit pins. This exposes the workflow to supply-chain attacks if the referenced tag is moved or the upstream repository is compromised.

Failing references:
- check-dist.yml: actions/checkout@v4, actions/setup-node@v3, actions/upload-artifact@v3
- ci.yml: actions/checkout@v4, actions/setup-node@v3
- sizeup.yaml: lerebear/sizeup-action@v0.8.2

Each should be pinned to a full 40-character hex SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/check-dist.yml:30`
- `.github/workflows/check-dist.yml:34`
- `.github/workflows/check-dist.yml:57`
- `.github/workflows/ci.yml:16`
- `.github/workflows/ci.yml:21`
- `.github/workflows/sizeup.yaml:14`

### missing-permissions (severity: medium)

The workflow file ci.yml has no top-level `permissions:` block and no job-level `permissions:` block on any of its jobs. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (e.g. write access to contents). A minimal permissions block such as `permissions: read-all` or specific scopes (e.g. `contents: read`) should be added.

Locations:

- `.github/workflows/ci.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all action references to full 40-character SHAs:
- check-dist.yml: actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262, actions/setup-node@v3 → @3235b876344d2a9aa001b8d1453c930bba69e610, actions/upload-artifact@v3 → @ff15f0306b3f739f7b6fd43fb5d26cd321bd4de5
- ci.yml: actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262, actions/setup-node@v3 → @3235b876344d2a9aa001b8d1453c930bba69e610
- sizeup.yaml: lerebear/sizeup-action@v0.8.2 → @ed1dfed124077e91d3736a1acfddded03aff4c91

Added top-level `permissions: contents: read` block to ci.yml to enforce least-privilege. The other two workflow files already had explicit permissions blocks.

