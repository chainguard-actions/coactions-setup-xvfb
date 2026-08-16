<!-- markdownlint-disable -->

# Hardening Report: coactions--setup-xvfb/v1.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **coactions--setup-xvfb/v1.0.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions and reusable workflows using mutable tags or branch names instead of pinned 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced tag or branch is updated with malicious code.

Failing references:
- ack.yml: `uses: ansible/devtools/.github/workflows/ack.yml@main`
- local.yml: `uses: actions/checkout@v3`
- prod.yml: `uses: actions/checkout@v3`, `uses: GabrielBB/xvfb-action@v1.6` (×3), `uses: GabrielBB/xvfb-action@v1`
- push.yml: `uses: ansible/devtools/.github/workflows/push.yml@main`
- release.yml: `uses: Actions-R-Us/actions-tagger@latest`

Locations:

- `.github/workflows/ack.yml:10`
- `.github/workflows/local.yml:14`
- `.github/workflows/prod.yml:11`
- `.github/workflows/prod.yml:13`
- `.github/workflows/prod.yml:18`
- `.github/workflows/prod.yml:23`
- `.github/workflows/prod.yml:28`
- `.github/workflows/push.yml:10`
- `.github/workflows/release.yml:14`

### missing-permissions (severity: medium)

None of the workflow files define a top-level `permissions:` key, and no job within any of these files defines job-level `permissions:` either. Without explicit permissions, workflows run with the default token permissions (which may be read/write depending on repository settings), granting broader access than necessary. All five workflow files are affected: ack.yml, local.yml, prod.yml, push.yml, and release.yml.

Locations:

- `.github/workflows/ack.yml:1`
- `.github/workflows/local.yml:1`
- `.github/workflows/prod.yml:1`
- `.github/workflows/push.yml:1`
- `.github/workflows/release.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 9 unpinned action references across 5 workflow files by pinning each to its full 40-character commit SHA (with original tag preserved as a comment). Added `permissions: {}` top-level block to all 5 workflow files (ack.yml, local.yml, prod.yml, push.yml, release.yml) to enforce least-privilege access. SHAs resolved: actions/checkout@v3→f43a0e5ff2bd294095638e18286ca9a3d1956744, GabrielBB/xvfb-action@v1.6 and @v1→86d97bde4a65fe9b290c0b3fb92c2c4ed0e5302d, ansible/devtools@main→b7222d7e2bd43e21b247fcdcb1a4014534637da0, Actions-R-Us/actions-tagger@latest→330ddfac760021349fef7ff62b372f2f691c20fb.

