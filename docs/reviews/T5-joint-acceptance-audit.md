# T5 joint acceptance evidence audit

Date: 2026-10-03 (Asia/Shanghai)

Authoritative pair under review:

- Core `4858520224a98b6528cb6cf47e7aa2e517bb62dc`
- Official UI `d77a9a92f7abd435c3d5447db8179e4325f3e79d`

This audit maps the ten T5 rows in the master development plan to independently verified evidence. `verified` means an exact-commit, reproducible path exists; `partial` means relevant behavior passed but the row lacks one explicit joint or release-form proof; `missing` means no adequate committed evidence exists yet.

| # | Acceptance row | Status | Existing exact evidence | Remaining proof |
|---|---|---|---|---|
| 1 | Fresh bare-Core install | verified | Platform fresh-archive wheel build/install and 104/104 installed tests at Core `c4fe126` | Consolidate in final release record |
| 2 | Complete CLI-only Core loop | verified | Fresh installed-package acceptance at Core `c4fe126` covers bare Core and the unknown-plugin lifecycle | None |
| 3 | CLI install and authorize Official UI | verified | Fresh UI U4 real-Core verifier installs, grants, enables, and starts the packaged UI | None |
| 4 | UI plugin and task management | verified | Real public reads/writes, confirmation, version/idempotency conflict and audit correlation passed | None |
| 5 | Core work continues after UI disable | verified | UI `c3a641f` proves the same unrelated resident plugin succeeds before and after UI disable, then remains healthy and callable after UI removal | None |
| 6 | UI uninstall leaves no authoritative UI state | verified | Disable/remove and Core CLI independence passed; UI has no private authority database | Record final state inspection in consolidated run |
| 7 | Core restart, UI restart, plugin crash and recovery | verified | UI/Core restart passed at UI `28f2428`; forced resident crash/recovery passed independently at Core `c4fe126` | None |
| 8 | Old protocol compatibility and incompatible rejection | verified | Explicit compatible alpha.1 fixture and incompatible rejection passed at Core `c4fe126`; UI incompatible bootstrap also rejects | None |
| 9 | M4/16 GB/256 GB target resource acceptance | verified with limitation | Actual target-class host ran 30/30 calls with 30 ledger rows, 0.361887 s wall time, 0.005850 s max call runtime and 23,805,952-byte harness peak RSS | Preserve the explicit no-long-soak/no-saturation limitation in the human review |
| 10 | Safe offline/default configuration | verified | All fresh acceptance runs were offline, local Unix socket/loopback only, with generated ephemeral credentials and no external service | Consolidate in final release record |

The current host is an Apple M4 Mac mini with 16 GB memory and a nominal 256 GB internal disk, so row 9 can be measured on the target class rather than extrapolated. Hardware serial and device identifiers are deliberately excluded from committed evidence.

## Release-operation documentation gap

F2 also requires version compatibility and install/upgrade/downgrade/uninstall procedures. Machine-readable compatibility rules and security limitations exist. Core `c4fe126` and UI `c3a641f` now have accepted matching lifecycle runbooks.

All ten functional rows are verified. Core `4858520` reports/builds `0.1.0-alpha.4`; the capability protocol remains `0.1.0-alpha.1`, the management protocol remains `0.1.0-alpha.3`, and the independently versioned Official UI remains `0.1.0-alpha.1`. UI `d77a9a9` pins that exact Core commit and the complete unchanged joint acceptance passes.

## Minimal completion sequence

1. Core and UI functional evidence lanes: completed and independently accepted at `c4fe126` / `c3a641f`.
2. Core package-version lane: completed and independently accepted at `4858520`.
3. UI evidence refresh: completed and independently accepted at `d77a9a9` against Core `4858520`.
4. Release candidate report: prepared; automatic progression stops for the mandatory human decision.

No release-candidate status is claimed by this audit.
