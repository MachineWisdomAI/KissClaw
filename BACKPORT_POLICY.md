# KissClaw Backport Policy

## D8 — Backport Eligibility

A cherry-pick onto a KissClaw release branch is eligible **only** when it
addresses a tracked production incident (Linear MK project). The originating
incident must be cited in the cherry-pick commit message.

The "recently closed upstream PRs" methodology is explicitly forbidden. The
ProdClaw POC demonstrated the failure mode: 21 backports, 0 originating MK
incidents addressed, 4 leaking test-import references, and an unbounded review
surface. Backport selection must be incident-driven, not recency-driven.

### Eligibility checklist

1. A Linear MK incident exists and is linked.
2. The upstream commit that fixes the incident is identified.
3. The fix applies cleanly to the current baseline (verified by
   `kc-check-imports`).
4. If the fix depends on supporting infrastructure not present in the baseline,
   the supporting commit is either:
   - small and self-contained (backport it first), or
   - part of a larger feature/refactor (the fix is ineligible; prefer D9).

### What is NOT eligible

- Upstream commits that "look useful" but have no incident.
- Feature commits, refactors, or cleanups.
- Commits whose supporting infrastructure requires a feature backport.

## D9 — Prefer Baseline Advancement

When multiple incidents can be resolved by advancing to a newer upstream
baseline (one that already contains the fixes), prefer advancing the baseline
over cherry-picking individual fixes.

Baseline advancement is a new `ga/X.Y` branch from a newer upstream tag. It
carries the full upstream diff and requires a full upstream intake review (see
`KISSCLAW.md`). It is preferable to cherry-picking when:

- 3 or more incidents would be resolved by the newer baseline;
- the newer baseline is at least 10 days old (GA eligibility);
- the cherry-pick set for the current baseline would exceed 10 commits.

## D10 — Test-Imports Pre-Check

Every cherry-pick must pass `kc-check-imports` validation before commit. The
tool verifies that the cherry-picked code does not reference symbols, modules, or
test helpers that do not exist in the current baseline.

The pre-commit hook provides fast local feedback. The CI workflow on `release/*`
PRs is the authoritative gate — hooks can be bypassed; CI cannot.
