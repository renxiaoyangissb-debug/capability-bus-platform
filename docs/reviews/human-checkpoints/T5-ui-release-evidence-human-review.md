# T5 Official UI release evidence — Platform verification

Date: 2026-10-03 (Asia/Shanghai)

## Candidate and disposition

- Previous verified UI commit: `28f2428130db4d39f546d34f20b440fbc72fc6d1`
- UI evidence candidate: `c3a641ffc22a21905753d68c2c87621c4354faee`
- Verified Core counterpart: `c4fe12634571154a9d64ce97f8f9922dc14317b3`
- Disposition: accepted by Platform as Official UI's final T5 release-evidence contribution
- UI runtime/contract change: none
- Release-candidate claim: none

The commit changes only the real-Core acceptance fixture and UI-owned release documentation. It does not change the UI runtime, packaged plugin implementation, dependency lock, or either frozen contract snapshot.

## Independent fresh-archive verification

Platform exported the exact candidate into `/tmp/capbus-ui-c3a641f.2MZNbO`, bootstrapped it offline, and verified it against the exact clean Core counterpart.

- Locked dependencies installed offline: 20
- `package-lock.json` SHA-256 before and after: `c8ef2875aa4c2ae0e7c0a62dd4c9c0081f6527b6ea6194c108f6f5a73bef38e5`
- Tests: 28/28 passed, zero skips
- Typecheck and production build: passed; 24 modules transformed
- License check: 20 installed packages passed
- Source/package-boundary checks: 17 source files passed
- Packaged plugin integrity: 31/31 files verified
- Frozen F1 and management contract directories: zero diff from Core
- Forbidden private-dependency/storage/network scan: zero matches
- `git diff --check`: passed
- Core, UI, and Platform worktrees: clean at final verification

The first sandboxed verification attempt was unable to bind the local Unix socket and failed one bridge test with `EPERM`. Platform reran the same fresh archive with only that local-socket sandbox restriction removed; the complete suite passed without a skipped or suppressed test.

## Independent non-UI continuity result

The real-Core fixture registered and started the unrelated `example.echo` process plugin before touching the UI lifecycle. Platform verified:

- the public invocation succeeded before UI disable;
- after UI disable, the UI URL was unreachable, Core CLI remained available, and the same resident non-UI process handled another successful invocation;
- after UI removal, the old UI URL remained unreachable, the unrelated plugin still reported `healthy` and resident, and another public invocation succeeded;
- UI pairing/session authority and process credentials did not survive the UI lifecycle boundary.

This closes T5 row 5 without granting the UI authority over Core or non-UI work.

## UI lifecycle documentation

The accepted runbook covers offline package construction and integrity, CLI installation/authorization/start, explicit Grant scope, state/config backup, compatibility-aware upgrade, backup-based downgrade, disable/remove, listener/session invalidation, and Core/non-UI independence. It prohibits direct Core database or credential-file edits.

## Resulting authorization boundary

Platform accepts UI `c3a641ffc22a21905753d68c2c87621c4354faee` for T5 evidence. All ten T5 functional acceptance rows now have reproducible evidence. The Core package still identifies itself as `0.1.0-alpha.3`; the roadmap's integrated Core release node is `0.1.0-alpha.4`. A package-version-only Core correction and matching UI exact-commit evidence refresh are required before the release-candidate human checkpoint.
