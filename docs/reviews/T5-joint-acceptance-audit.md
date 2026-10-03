# T5 joint acceptance evidence audit

Date: 2026-10-03 (Asia/Shanghai)

Authoritative pair under review:

- Core `c4fe12634571154a9d64ce97f8f9922dc14317b3`
- Official UI `28f2428130db4d39f546d34f20b440fbc72fc6d1`

This audit maps the ten T5 rows in the master development plan to independently verified evidence. `verified` means an exact-commit, reproducible path exists; `partial` means relevant behavior passed but the row lacks one explicit joint or release-form proof; `missing` means no adequate committed evidence exists yet.

| # | Acceptance row | Status | Existing exact evidence | Remaining proof |
|---|---|---|---|---|
| 1 | Fresh bare-Core install | verified | Platform fresh-archive wheel build/install and 104/104 installed tests at Core `c4fe126` | Consolidate in final release record |
| 2 | Complete CLI-only Core loop | verified | Fresh installed-package acceptance at Core `c4fe126` covers bare Core and the unknown-plugin lifecycle | None |
| 3 | CLI install and authorize Official UI | verified | Fresh UI U4 real-Core verifier installs, grants, enables, and starts the packaged UI | None |
| 4 | UI plugin and task management | verified | Real public reads/writes, confirmation, version/idempotency conflict and audit correlation passed | None |
| 5 | Core work continues after UI disable | partial | UI listener disappears and Core CLI remains usable after disable | Demonstrate an already-running or durable non-UI plugin operation remains correct across UI disable/removal |
| 6 | UI uninstall leaves no authoritative UI state | verified | Disable/remove and Core CLI independence passed; UI has no private authority database | Record final state inspection in consolidated run |
| 7 | Core restart, UI restart, plugin crash and recovery | verified | UI/Core restart passed at UI `28f2428`; forced resident crash/recovery passed independently at Core `c4fe126` | None |
| 8 | Old protocol compatibility and incompatible rejection | verified | Explicit compatible alpha.1 fixture and incompatible rejection passed at Core `c4fe126`; UI incompatible bootstrap also rejects | None |
| 9 | M4/16 GB/256 GB target resource acceptance | verified with limitation | Actual target-class host ran 30/30 calls with 30 ledger rows, 0.361887 s wall time, 0.005850 s max call runtime and 23,805,952-byte harness peak RSS | Preserve the explicit no-long-soak/no-saturation limitation in the human review |
| 10 | Safe offline/default configuration | verified | All fresh acceptance runs were offline, local Unix socket/loopback only, with generated ephemeral credentials and no external service | Consolidate in final release record |

The current host is an Apple M4 Mac mini with 16 GB memory and a nominal 256 GB internal disk, so row 9 can be measured on the target class rather than extrapolated. Hardware serial and device identifiers are deliberately excluded from committed evidence.

## Release-operation documentation gap

F2 also requires version compatibility and install/upgrade/downgrade/uninstall procedures. Machine-readable compatibility rules and security limitations exist. Core `c4fe126` now has an accepted lifecycle runbook; the matching Official UI lifecycle runbook remains missing. This is an evidence/documentation gap, not authorization for a new product feature or contract change.

## Minimal completion sequence

1. Core lane: completed and independently accepted at `c4fe126`.
2. Official UI lane: add the smallest joint continuation proving non-UI work survives UI disable/removal and document UI install/upgrade/downgrade/uninstall behavior.
3. Platform runs one final consolidated acceptance pass, prepares `T5-release-candidate-human-review.md`, and stops for the mandatory human decision.

No release-candidate status is claimed by this audit.
