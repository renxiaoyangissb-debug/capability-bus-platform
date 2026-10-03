# T5 Official UI real-Core integration — Platform verification

Date: 2026-10-03 (Asia/Shanghai)

## Candidate and disposition

- F2 Official UI baseline: `39b5904499c0ecfa0dff1d57683812c150e377e4`
- Official UI U4 candidate: `28f2428130db4d39f546d34f20b440fbc72fc6d1`
- Verified Core counterpart: `322503845cb322628fe2db7a2c570fc6289d22db`
- Disposition: accepted by Platform as the verified UI-U4 real-Core integration candidate
- Release-candidate claim: none

The candidate replaces the browser's fake-only runtime path with a packaged local bridge to Core's public management protocol while retaining the fake transport for deterministic tests. It does not change either frozen contract family.

## Independent fresh-archive verification

Platform exported the exact UI candidate into `/tmp/capbus-ui-28f2428.p6jXJN` and verified it against the exact Core counterpart without external network access or real credentials.

- Deterministic offline bootstrap: 20 locked packages installed on `darwin/arm64`
- `package-lock.json` SHA-256 before and after bootstrap: `c8ef2875aa4c2ae0e7c0a62dd4c9c0081f6527b6ea6194c108f6f5a73bef38e5`
- TypeScript tests: 28/28 passed, zero skips
- Production build: passed; 24 modules transformed
- Dependency-license check: 20 installed packages passed
- Source and package-boundary verification: 17 source files passed
- Packaged plugin integrity: 31/31 files verified
- `git diff --check`: passed

Platform also bypassed the UI repository's own provenance checks and independently compared the exported trees:

- Core `contracts/v0.1/` versus UI frozen F1 contracts: zero directory diff
- Core `contracts/management-v0.1/` versus UI management contracts: zero directory diff
- UI source, plugin source, and packaged output forbidden-dependency scan: zero matches for Core Python imports, SQLite access, owner-key access, browser storage, or wildcard network binding

## Real-Core lifecycle result

The candidate's real integration verifier packaged the UI and drove a temporary, isolated Core instance through the public CLI, supervised-plugin, Capability, and management paths. The run passed all of the following:

- package install, authorization, enable, and supervised start;
- public read and controlled write operations;
- confirmation, expected-version conflict, idempotency hit, and idempotency conflict behavior;
- audit correlation;
- immediate failure after management Grant revocation;
- disable and removal without retaining UI-owned authoritative state;
- UI restart and Core restart recovery;
- incompatible-version rejection.

The verifier used a local Unix socket and loopback HTTP only. It did not use external services, production credentials, Core private databases, or an owner key in the browser/plugin process.

## Security assessment and Alpha limitations

- Core injects the scoped plugin identity and short-lived management credential only into the supervised backend process.
- The browser receives a one-use pairing flow followed by a short-lived, in-memory, `HttpOnly`, `SameSite=Strict` session cookie; it never receives the management HMAC key.
- The bridge validates Origin, CSRF, request size, required bootstrap variables, and local-only binding, and emits defensive response headers.
- Fake transport remains test-only; production packaging selects the real bridge.
- Alpha still does not claim an OS sandbox against another malicious process running as the same user.
- Loopback transport is plain HTTP and therefore does not use a `Secure` cookie; the boundary is local-only and must not be exposed beyond loopback.
- Sessions are intentionally in memory and require pairing again after a bridge restart.

## Resulting authorization boundary

Platform accepts Official UI `28f2428130db4d39f546d34f20b440fbc72fc6d1` against Core `322503845cb322628fe2db7a2c570fc6289d22db` for UI-U4. T5 remains open for joint release-evidence consolidation, including the explicit CLI-only, crash/recovery and old-protocol matrix, target-machine resource acceptance, and install/upgrade/downgrade/uninstall documentation review. Publication or labeling of a release candidate remains blocked on the mandatory human checkpoint.
