# T4 Core Alpha.3 Platform verification

Date: 2026-10-03 (Asia/Shanghai)

## Disposition

`CORE-T4 / 0.1.0-alpha.3` is verified by Platform at Core commit:

`272f6325506c4ce7c4de5c100fb3f03a28701318`

This verification authorizes UI U3 to consume the exact public management contract candidate from that commit. It does not enter or accept F2.

## Candidate history

The initial candidate `8a77b747ff8b5bd8f824157a926b1c8f69bf2785` passed its runtime suite but was rejected because its machine contract did not enumerate individual operations, CLI high-impact confirmation was implicit, and declared alpha.3 controls lacked direct tests.

Corrective commit `272f6325506c4ce7c4de5c100fb3f03a28701318` added the per-operation contract catalog, explicit CLI confirmation, audited and redacted public failures, conformance examples, and direct feature coverage.

## Independent verification

Platform verified from a clean Git archive rather than the owning conversation's working tree:

- built `capability_bus_core-0.1.0a3-py3-none-any.whl`;
- installed the wheel into a fresh temporary target;
- ran the complete installed suite: **100/100 passed, zero skipped**;
- Unix-socket daemon tests passed outside the sandbox restriction;
- compiled `src`, `tests`, and `system_plugins` successfully;
- parsed every `management-v0.1` schema, catalog, and example as JSON;
- verified `contracts/v0.1` has zero diff from T3 baseline `d3556226ec6e8221781e4267ca2aa0b6ca523ebd`;
- verified the Core worktree remained clean;
- independently built wheel SHA-256: `0f4a3c507268b47c703b3b4440fa4edf3e0e40c8839d3db14417e5b7eef89843`.

## Accepted contract and security evidence

- The operation catalog is machine-readable and tested against the runtime operation policy.
- Write operations declare idempotency, confirmation, expected-version, argument, result, scope, and stable error requirements.
- CLI high-impact operations require an explicit confirmation flag; implicit confirmation is rejected.
- Credential rotation invalidates old credentials.
- Grant expiry and workspace/SecretRef scopes fail closed.
- Trust Zone changes, configuration concurrency/history/rollback, audit retention, package rollback integrity, and SecretRef-only boundaries have direct tests.
- Public failures are stable and redacted, and safely parsed denials/errors return audit correlation.
- Durable Task/Event state remains owned by the system plugin; no product-specific workflow entered bare Core.

## Remaining gate

UI U3 must implement controlled write interaction solely against this exact public contract baseline. After independent UI U3 verification, Platform must prepare the F2 report and stop for human acceptance.
