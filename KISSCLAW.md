# KissClaw

KissClaw is a production-stability downstream of OpenClaw. It exists to make
OpenClaw usable for production operators who need slower intake, safer defaults,
repeatable releases, and explicit maturity channels.

KissClaw is not a private deployment product. Keep customer-specific operations,
private bridges, fleet topology, hostnames, tenant names, and managed-service
runbooks out of this repository.

## Boundary

KissClaw owns generic runtime stability:

- curated upstream intake from OpenClaw;
- safer default behavior for customer-visible channels;
- source-level review of config schema and default changes;
- package acceptance and release provenance;
- GA and LTS release channels.

Private deployments built on KissClaw own their own infrastructure:

- cloud hosts, images, bastions, and fleet rollout;
- tenant lifecycle and isolation;
- private OAuth/account bridges;
- customer-specific prompts, policies, and support workflows;
- operational monitoring and incident runbooks.

If a change names a private deployment, customer, host, or bridge, it belongs in
that deployment repository, not in KissClaw. If a change makes OpenClaw safer or
more predictable for any production operator, it can belong here.

## Branch Model

KissClaw uses a **governance-patch** branch model:

- `main` mirrors upstream OpenClaw at the chosen baseline + the release workflow.
- `governance-patch` carries all governance documents, CODEOWNERS, and CI
  workflows as a permanently-open patch series rebased onto `main`.
- `ga/X.Y` release branches are cut from `governance-patch` (not from `main`).
- Release candidates and tags are cut from `ga/X.Y` branches.

Maintainers force-push `governance-patch` using `--force-with-lease` only.
GitHub branch protection allows force-push by selected maintainers but cannot
enforce the lease semantics — that is operator discipline.

## Release Channels

KissClaw has two maturity channels.

**GA** is the current production-ready channel. GA releases are cut at most every
two weeks, and only from upstream OpenClaw releases that are at least 10 days
old. A GA release may be delayed when community signal or local evidence shows
regression risk.

**LTS** is the conservative channel. LTS releases are promoted quarterly from a
proven GA release and receive only security fixes and critical regression
backports. LTS is for operators who value stability over new OpenClaw features.

KissClaw uses SemVer tags:

- `vMAJOR.MINOR.PATCH` for GA and LTS releases;
- `vMAJOR.MINOR.PATCH-rc.N` for release candidates.

KissClaw release tags start at major version 1. Do not use upstream OpenClaw
date versions as KissClaw versions. Record upstream OpenClaw provenance in
release metadata instead. Keep `KISSCLAW_UPSTREAM.json` current so release
artifacts carry the upstream package version that the KissClaw release is based
on.

## Key Policies

- **D7 Release-Workflow Invariant**: release builds must come from a tag or
  release branch, never from the upstream mirror (`main`). The release workflow
  enforces this with a checkout-integrity step.
- **D8 Backport Eligibility**: see `BACKPORT_POLICY.md`.
- **D9 Prefer Baseline Advancement**: see `BACKPORT_POLICY.md`.
- **D10 Test-Imports Pre-Check**: cherry-picks are validated by `kc-check-imports`
  before commit.

## Upstream Intake

Every upstream intake starts as an explicit PR. The PR must include:

- upstream OpenClaw version, tag, commit, and release date;
- confirmation that the upstream release is at least 10 days old;
- changelog summary and source diff summary;
- config schema/default diff for high-risk runtime surfaces;
- package contents diff;
- community regression scan;
- GA or LTS impact statement.
