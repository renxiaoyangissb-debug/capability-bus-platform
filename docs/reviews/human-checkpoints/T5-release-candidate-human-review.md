# T5 / alpha.4 release candidate — human review

Date: 2026-10-03 (Asia/Shanghai)

## Decision requested

Accept or reject the exact T5 / alpha.4 release-candidate pair:

- Core: `4858520224a98b6528cb6cf47e7aa2e517bb62dc`
  - distribution `0.1.0a4`
  - runtime/CLI `0.1.0-alpha.4`
- Official UI: `d77a9a92f7abd435c3d5447db8179e4325f3e79d`
  - independently versioned package `0.1.0-alpha.1`

No release-candidate acceptance is recorded until the user explicitly accepts this checkpoint.

## Frozen compatibility baseline

- Capability/Manifest/Envelope protocol remains `0.1.0-alpha.1` and is byte-identical to accepted F1.
- Public management protocol remains `0.1.0-alpha.3` with the same 42-operation machine-readable catalog and is byte-identical to the verified F2/T5 authority.
- Core alpha.4 is a package/runtime integration version, not a protocol-version change.
- UI uses only the public Capability and management paths; it imports no Core package, reads no Core database/private directory, holds no owner key, and owns no authoritative Core state.

## Independently verified release evidence

### Core

- Fresh-archive offline wheel: `capability_bus_core-0.1.0a4-py3-none-any.whl`
- Platform wheel SHA-256: `fb604d036a69f284562fdf2b52dc20a51402f32a3fe56407b16f37039aa91211`
- Wheel boundary: 27 expected Core/SDK/distribution files
- Installed suite: 105/105 passed, zero skips, including Unix sockets
- Installed-package compilation and `git diff --check`: passed
- Bare-Core and unknown-plugin CLI lifecycle: passed
- Compatible legacy alpha.1 plugin: accepted
- Incompatible protocol: rejected fail-closed
- Forced resident-plugin crash/recovery: passed
- Offline operation with generated ephemeral credentials: passed

### Official UI and joint integration

- Fresh-archive deterministic offline install: 20 locked packages
- Lock SHA-256 before/after: `c8ef2875aa4c2ae0e7c0a62dd4c9c0081f6527b6ea6194c108f6f5a73bef38e5`
- Tests: 28/28 passed, zero skips
- Typecheck, 24-module production build, 20-package license check and 17-file boundary check: passed
- Packaged plugin integrity: 31/31 files verified
- Exact F1 and management contract directory comparisons: zero diff
- Real Core lifecycle: install, authorize, enable/start, public reads/writes, confirmation, optimistic version conflict, idempotency hit/conflict, audit correlation, Grant revocation, UI/Core restart, disable/remove and incompatibility rejection all passed
- Independent non-UI resident plugin: same process remained callable after UI disable; remained healthy/resident and callable after UI removal
- UI listener and session authority: unavailable after disable/removal while Core CLI remained usable

## T5 acceptance matrix

All ten rows in the master plan have reproducible evidence:

1. fresh bare-Core install;
2. CLI-only Core loop;
3. CLI install/authorization of Official UI;
4. UI plugin and task/control management through public operations;
5. non-UI work continuity after UI disable;
6. UI removal without UI-owned authoritative state;
7. Core restart, UI restart and plugin crash/recovery;
8. legacy compatibility and incompatible rejection;
9. target-class resource acceptance;
10. offline/default operation without real credentials.

The target-host resource run used an actual Apple M4 / 16 GB / nominal 256 GB machine: 30/30 resident calls succeeded, 30 resource rows were recorded, wall time was 0.453376 seconds, maximum recorded call runtime was 0.008355 seconds, and harness peak RSS was 23,969,792 bytes.

## Lifecycle and operator evidence

Core and UI runbooks now cover fresh offline installation, integrity verification, explicit Grants, backup-before-upgrade, copy-first/compatibility checks, backup-based downgrade, disable/remove, state-preserving code uninstall, explicit purge boundaries, and fail-closed handling of unknown state compatibility. Direct SQLite or private credential edits are prohibited.

## Known Alpha limitations

- Process separation is not an OS sandbox against another malicious process running as the same user.
- The resource test is bounded acceptance, not a long-duration soak, saturation or thermal test.
- The browser bridge is loopback-only plain HTTP; it has no remote-access/TLS claim and therefore does not use a `Secure` cookie. Pairing, `HttpOnly`, `SameSite=Strict`, Origin/CSRF, CSP/frame denial and in-memory expiry remain enforced.
- Browser sessions are in memory and require pairing again after restart.
- There is no package-signing trust chain, automatic remote backup or production deployment claim.
- The optional MCP interoperability gate is not included in this candidate; the current master plan does not make it a T5 release blocker.

## Required response

- Accept: `接受发布候选`
- Reject: identify the blocking release, security, compatibility, lifecycle, or resource concern.

Acceptance records this exact pair as the T5 / alpha.4 release candidate. It does not by itself authorize public package upload, external deployment, production credentials, public network exposure, or a breaking protocol change.

## Human acceptance record

On 2026-10-03 (Asia/Shanghai), the user replied:

```text
接受发布候选
```

This accepts Core `4858520224a98b6528cb6cf47e7aa2e517bb62dc` and Official UI `d77a9a92f7abd435c3d5447db8179e4325f3e79d` as the exact T5 / alpha.4 release-candidate pair, with the frozen protocol versions, verified evidence and known Alpha limitations recorded above.

This acceptance does not authorize public package upload, external deployment, production credentials, public network exposure, destructive state migration, or a breaking protocol change. Those actions require separate explicit scope and authorization.
