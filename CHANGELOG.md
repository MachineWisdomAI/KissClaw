# KissClaw Changelog

## KissClaw 1.1.0-rc.1

Baseline upgraded: OpenClaw v2026.4.20 → v2026.4.23 (`a9797214338b`).

### Changes

- Upstream baseline upgraded from v2026.4.20 to v2026.4.23 (1,277 upstream commits).
- 3 original downstream fixes auto-dropped as already-upstream at v2026.4.23.
- 0 new post-v2026.4.23 cherry-picks (all 5 candidates conflict due to module reorganization between v2026.4.23 and upstream/main; deferred to next baseline upgrade).

---

## KissClaw 1.0.1-rc.3

Baseline: OpenClaw v2026.4.20 (`8116e638f3`).

Drop-in upstream compatibility fix. No code changes.

### Changes

- fix(governance): restore `bin: openclaw` for upstream drop-in compatibility
  - The rc.2 tarball installed a `kissclaw` binary instead of `openclaw`,
    breaking compatibility with existing configs, LaunchAgents, and shell
    history that reference `openclaw`.
- docs: update README banner with install instructions and migration steps

## KissClaw 1.0.1-rc.2

Baseline: OpenClaw v2026.4.20 (`8116e638f3`).

Same fix set as 1.0.1-rc.1 plus the build-unblocking type extension below.
The 1.0.1-rc.1 release artifacts failed to build because of this type
mismatch and never published tarballs; this RC supersedes it.

### Fixes

- fix(agents): extend onAgentEvent callback type with sessionKey (triage)
  - Type-only extension required by the lifecycle backstop cherry-pick.
  - Subset of upstream `1184925572` (excluded under D8 as a CI-perf
    refactor); only the type signature is included.

## KissClaw 1.0.1-rc.1

Baseline: OpenClaw v2026.4.20 (`8116e638f3`).

24 upstream cherry-picks validated by kc-check-imports against the v2026.4.20
baseline. Each fix addresses code present at the baseline or includes its
required supporting backport.

### Fixes

- fix(security): stop implicit tool grants from config sections (#47487, #75055)
- fix(gateway): align sessions abort wait semantics (#74751)
- fix(cron): preserve model overrides for text payloads (#73946)
- fix(exec): preserve turnSourceChannel as messageProvider in approval followup runs (#74666)
- fix(feishu): skip empty-text messages with no media to prevent blank session turns (#74634, #74661)
- fix(security): bound bootstrap handoff scopes (#72919)
- fix(security): remediate CodeQL alerts
- fix(device-pair): reject invalid remote setup URLs
- fix: gate startup context for sandboxed spawned sessions (#73611)
- fix(gateway): preserve rpc abort terminal snapshots
- fix: environment edge case launcher regression (#74696)
- fix(agents): finalize embedded lifecycle backstop
- fix(agents): preserve string user content when merging turns
- fix: derive dynamic context-window guard thresholds
- fix: reject invalid cron edits on disabled jobs (#74720)
- fix(cron): catch croner parse errors in add/update handlers (#74193)
- fix: accept previously documented WhatsApp exposeErrorText key (#74667)
- fix: interpolate heartbeat response prefix templates (#73996)
- fix(acp): fall through to thread-bound resolution on unresolvable token (#66299, #74641)
- fix(mattermost): add WebSocket ping/pong keepalive (#73979)
- fix(cron): accept threaded delivery in gateway schema
- fix(outbound): hold active-delivery claim so reconnect drain skips live sends
- fix: isolate cron context-engine session keys (#72292)
- fix(cron): preserve current delivery target context

### Triage (baseline compatibility)

- fix(gateway): import isAbortError in agent.ts
- fix(gateway): remove dead refs to upstream task-tracking helpers
- test: skip post-baseline tests across 11 test files
