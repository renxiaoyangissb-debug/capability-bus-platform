# T5 Core release evidence — Platform verification

Date: 2026-10-03 (Asia/Shanghai)

## Candidate and disposition

- Previous verified Core T5 commit: `322503845cb322628fe2db7a2c570fc6289d22db`
- Core evidence candidate: `c4fe12634571154a9d64ce97f8f9922dc14317b3`
- Disposition: accepted by Platform as Core's T5 release-evidence contribution
- Runtime/contract change: none
- Release-candidate claim: none

The commit adds only an offline acceptance harness, two focused tests, a lifecycle runbook, verification evidence, and README links. It does not change Core or SDK runtime sources, package configuration, or either frozen contract tree.

## Independent fresh-archive verification

Platform exported the exact candidate into `/tmp/capbus-core-c4fe126.iBRF1l`, built and installed the wheel without dependency resolution or network access, and ran all verification against the installed package.

- Wheel: `capability_bus_core-0.1.0a3-py3-none-any.whl`
- Independent wheel SHA-256: `a8bb2b15c4b73ea97027a19a64f8605a21399a59eb885370fe0a8e0f03b48dbd`
- Wheel boundary: 27 Core/SDK/distribution files; no UI, source baseline, tests, product flow, or system-plugin implementation
- Complete installed suite: 104/104 passed, zero skips
- Compilation of installed package and tests: passed
- `git diff --check`: passed
- Core, UI, and Platform worktrees: clean at final verification
- F1 and F2 contract trees: unchanged

Platform ran the acceptance harness itself. Its installed-package CLI path passed the empty bare-Core checks and the complete unknown-plugin register, grant, enable, start, invoke, disable, confirmed-remove loop. The explicit legacy `0.1.0-alpha.1` fixture was accepted; an incompatible `0.1.0-alpha.999` fixture failed closed. A forced resident `crash-once` process recovered under a different runtime instance and the next invocation succeeded.

## Target-host resource result

The independent run executed on the actual target class: Apple M4, 16 GB memory, nominal 256 GB internal storage. Hardware serial and device identifiers were not collected.

| Measure | Independent result | Bound |
|---|---:|---:|
| Successful sequential resident calls | 30/30 | 30/30 |
| Resource-ledger rows | 30 | one per call |
| Workload wall time | 0.361887 s | < 30 s |
| Maximum recorded call runtime | 0.005850 s | < 2 s |
| Total recorded call runtime | 0.059940 s | informational |
| Peak acceptance-harness RSS | 23,805,952 bytes | informational, well below 16 GB |
| Observed mounted filesystem | 245,107,195,904 bytes total; 182,443,065,344 bytes free | target-class capacity confirmed |

This bounded Alpha acceptance demonstrates correct resource accounting and substantial headroom. It is not a long-duration soak, saturation, thermal, or OS-sandbox claim; that limitation must remain visible in the release-candidate review.

## Lifecycle documentation

The accepted Core runbook documents offline fresh install, full-root owner-only backup before upgrade, copy-first verification of forward-only migrations, backup-based downgrade, state-preserving code uninstall, explicit state purge, and fail-closed behavior when state compatibility is unknown.

## Resulting authorization boundary

Platform accepts Core `c4fe12634571154a9d64ce97f8f9922dc14317b3` for T5 evidence. T5 still requires the Official UI release-evidence slice proving non-UI work survives UI disable/removal and documenting UI install/upgrade/downgrade/uninstall behavior. Release-candidate publication remains blocked on the mandatory human checkpoint.
