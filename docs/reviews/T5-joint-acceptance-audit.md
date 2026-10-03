# T5 joint acceptance evidence audit

Date: 2026-10-03 (Asia/Shanghai)

Authoritative pair under review:

- Core `322503845cb322628fe2db7a2c570fc6289d22db`
- Official UI `28f2428130db4d39f546d34f20b440fbc72fc6d1`

This audit maps the ten T5 rows in the master development plan to independently verified evidence. `verified` means an exact-commit, reproducible path exists; `partial` means relevant behavior passed but the row lacks one explicit joint or release-form proof; `missing` means no adequate committed evidence exists yet.

| # | Acceptance row | Status | Existing exact evidence | Remaining proof |
|---|---|---|---|---|
| 1 | Fresh bare-Core install | verified | Platform fresh-archive wheel build/install and 102/102 installed tests at Core `3225038` | Consolidate in final release record |
| 2 | Complete CLI-only Core loop | partial | Accepted F1 CLI scenarios remain in the exact candidate's full regression suite | Record a fresh installed-package transcript for bare Core and unknown plugin at `3225038` |
| 3 | CLI install and authorize Official UI | verified | Fresh UI U4 real-Core verifier installs, grants, enables, and starts the packaged UI | None |
| 4 | UI plugin and task management | verified | Real public reads/writes, confirmation, version/idempotency conflict and audit correlation passed | None |
| 5 | Core work continues after UI disable | partial | UI listener disappears and Core CLI remains usable after disable | Demonstrate an already-running or durable non-UI plugin operation remains correct across UI disable/removal |
| 6 | UI uninstall leaves no authoritative UI state | verified | Disable/remove and Core CLI independence passed; UI has no private authority database | Record final state inspection in consolidated run |
| 7 | Core restart, UI restart, plugin crash and recovery | partial | UI and Core restart/reprovision passed; Core crash/recovery suites passed at exact commit | Add an explicit forced plugin-crash/recovery run in the joint candidate context |
| 8 | Old protocol compatibility and incompatible rejection | partial | Frozen contracts are byte-identical and incompatible UI bootstrap rejection passed | Run an explicit compatible legacy-protocol plugin fixture at the exact candidate |
| 9 | M4/16 GB/256 GB target resource acceptance | missing | Resource-governor unit tests and measurements exist, but no target-host acceptance record | Measure the candidate on the actual target-class host with bounded repeatable workload and record budgets/results |
| 10 | Safe offline/default configuration | verified | All fresh acceptance runs were offline, local Unix socket/loopback only, with generated ephemeral credentials and no external service | Consolidate in final release record |

The current host is an Apple M4 Mac mini with 16 GB memory and a nominal 256 GB internal disk, so row 9 can be measured on the target class rather than extrapolated. Hardware serial and device identifiers are deliberately excluded from committed evidence.

## Release-operation documentation gap

F2 also requires version compatibility and install/upgrade/downgrade/uninstall procedures. Machine-readable compatibility rules and security limitations exist, but the exact joint candidate does not yet have a consolidated Core release runbook or matching Official UI lifecycle runbook. These are evidence/documentation gaps, not authorization for new product features or a contract change.

## Minimal completion sequence

1. Core lane: exact-candidate installed CLI transcript, compatible legacy fixture, forced crash/recovery, target-host resource measurements, and Core lifecycle runbook.
2. Platform independently verifies that Core evidence and accepts or issues one scoped correction.
3. Official UI lane: add the smallest joint continuation proving non-UI work survives UI disable/removal and document UI install/upgrade/downgrade/uninstall behavior.
4. Platform runs one final consolidated acceptance pass, prepares `T5-release-candidate-human-review.md`, and stops for the mandatory human decision.

No release-candidate status is claimed by this audit.
