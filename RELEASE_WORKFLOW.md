# KissClaw Release Workflow

## D7 — Release-Workflow Invariant

Release builds **must** come from a tag or release branch, never from the
upstream mirror branch (`main`). The `kissclaw-release.yml` workflow enforces
this with a checkout-integrity step that:

1. Resolves the intended ref to a commit SHA (handling annotated tags correctly
   via `git rev-list -n 1`).
2. Verifies the actual checked-out HEAD matches the intended SHA.
3. Fetches the default-branch HEAD via the GitHub API (not assuming
   `origin/main` exists locally) and verifies it differs from the checkout.
4. Fails the build if any check fails.

### Why

A release built from `main` would produce an artifact with the raw upstream
tree — no governance docs, no CODEOWNERS, no KissClaw metadata. The D7
invariant makes this structurally impossible.

## Release Procedure

### Cutting a release candidate

1. Ensure `governance-patch` is rebased onto the desired `main` baseline.
2. Create or advance `ga/X.Y` from `governance-patch`.
3. Cherry-pick incident-driven fixes onto `release/vX.Y.Z-rc.N` (from `ga/X.Y`).
4. Each cherry-pick must pass `kc-check-imports` (see `BACKPORT_POLICY.md` D10).
5. Bump `package.json` version to `X.Y.Z-rc.N`.
6. Write `CHANGELOG.md` for the RC (only actually-included fixes).
7. Open PR: `release/vX.Y.Z-rc.N` -> `ga/X.Y`.
8. CI runs `kc-check-imports` per-candidate AND `--final-tree`.
9. Merge, create annotated tag `vX.Y.Z-rc.N`, push tag.
10. Create GitHub release (prerelease) — triggers `kissclaw-release.yml`.

### Verifying a release (Phase 6 gate)

After the release workflow completes:

1. Inspect the "Verify checkout integrity" step in the workflow run logs.
2. Confirm the logged SHA matches `git rev-list -n 1 vX.Y.Z-rc.N`.
3. Download the tarball via `gh release download`.
4. Verify `tar -tzf` lists governance files (KISSCLAW.md, CODEOWNERS, etc.).
5. Verify `package.json` version inside the tarball is `X.Y.Z-rc.N`.
6. Verify `package.json` name is `kissclaw`.
7. Test install: `npm install -g ./kissclaw-X.Y.Z-rc.N.tgz`.
8. Verify `kissclaw --version` reports `X.Y.Z-rc.N`.

### Promoting to GA

1. Verify the RC has had sufficient soak time (minimum 48 hours).
2. Retag or cut a new tag without the `-rc.N` suffix.
3. Create a non-prerelease GitHub release.
4. Update `KISSCLAW_UPSTREAM.json` if the baseline changed.
