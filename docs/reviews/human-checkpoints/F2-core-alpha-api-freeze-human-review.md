# F2 Core Alpha API Freeze — human review

Date: 2026-10-03 (Asia/Shanghai)

## Decision requested

Accept or reject the F2 Core Alpha API Freeze candidate consisting of:

- Core: `272f6325506c4ce7c4de5c100fb3f03a28701318`
- Official UI: `39b5904499c0ecfa0dff1d57683812c150e377e4`

No F2 acceptance is recorded until the user explicitly accepts this checkpoint.

## What is frozen

- Accepted F1 invocation and read contracts remain unchanged.
- Management protocol `0.1.0-alpha.3` and its 42-operation machine-readable catalog.
- Request/response, authorization, stable error, idempotency, expected-version, confirmation, and audit-correlation semantics.
- Persistent identity and credential rotation, resource-scoped Grants, Trust Zones, versioned configuration and rollback, SecretRef boundaries, audit retention, provider replacement and package rollback semantics.
- Official UI controlled-write behavior bound to the exact Core contract snapshot.

## Verified evidence

Core evidence is detailed in [T4 verification](T4-core-alpha3-human-review.md):

- fresh-archive wheel build and isolated install;
- 100/100 tests passed with no skips, including Unix sockets;
- compilation and F1 zero-diff checks passed;
- explicit confirmation, audited redacted failures, contract/runtime consistency, security scopes, migration and rollback coverage passed.

UI evidence is detailed in [U3 verification](U3-controlled-writes-human-review.md):

- deterministic fresh-tree offline installation with unchanged lock hash;
- exact 12-file management contract provenance;
- 26/26 tests, TypeScript, 27-module build, licenses and boundaries passed;
- packaged interface plugin integrity verified 31/31 files;
- no real Core or private-state access.

## Compatibility rule after acceptance

After F2 acceptance, T5 / alpha.4 may perform real Core/UI integration, installation, lifecycle, removal, recovery, compatibility, and resource testing. Breaking changes to the frozen Alpha API require explicit versioning, migration evidence, and a new human decision; alpha.4 is not a scope-expansion phase.

## Required response

- Accept: `接受 F2`
- Reject: identify the blocking contract, security, compatibility, or interaction concern.
