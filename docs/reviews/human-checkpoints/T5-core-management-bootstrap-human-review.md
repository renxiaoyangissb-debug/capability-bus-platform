# T5 Core management bootstrap — Platform verification

Date: 2026-10-03 (Asia/Shanghai)

## Candidate and disposition

- F2 Core baseline: `272f6325506c4ce7c4de5c100fb3f03a28701318`
- Core compatibility candidate: `322503845cb322628fe2db7a2c570fc6289d22db`
- Disposition: accepted by Platform for resuming Official UI U4 integration
- Release-candidate claim: none

The candidate adds the missing public, scoped management bootstrap for supervised resident interface plugins. It does not change the names, arguments, results, confirmation, idempotency, expected-version, error, or audit semantics of the 42 F2 management operations.

## Independent fresh-archive verification

Platform exported the exact candidate commit into `/tmp/capbus-core-3225038.gcmeFw`, built and installed it without dependencies or network access, and ran tests against the isolated installed package.

- Wheel: `capability_bus_core-0.1.0a3-py3-none-any.whl`
- Wheel SHA-256: `84a028fe05bcc0ac1ac490848471ee36c8bff66368fa8362c3338a70edb825bb`
- Wheel boundary: 27 Core/SDK/distribution files; no UI, product flow, source baseline, test, or system-plugin package included
- Candidate bootstrap tests: 2/2 passed
- Complete installed suite: 102/102 passed, zero skips; Unix-domain socket tests executed
- Compilation: passed for the isolated installed package and tests
- `git diff --check`: passed
- All contract JSON: parsed successfully

## Contract integrity

- `contracts/v0.1/`: zero diff from F2 baseline
- `contracts/management-v0.1/`: only `README.md` changed to document the additive interface-plugin bootstrap
- The operation catalog, request/response schemas, examples, and all other machine-readable management files: zero diff

## Original-blocker reproduction

Platform added a temporary acceptance-only interface plugin in the fresh archive. The fixture imports no Core package and reads no Core database or private directory. It was registered, granted, enabled, started under the real resident supervisor, and invoked through the normal Capability path.

The 1/1 end-to-end acceptance test verified:

- Core injected a stable plugin identity, independent session credential, management protocol version, and public Unix-socket location;
- the supervised plugin signed `system.status.get` itself and received a successful response through the real local-control management envelope;
- no owner-key environment variable was present and the plugin identity was not `owner:local`;
- revoking the management Grant caused the still-running process's old credential to fail immediately with `CREDENTIAL_INVALID`.

## Security assessment

- Management identity is restricted to `kind=interface` and the stable plugin system identity.
- Manifest `consumes` is declaration only; an active, unexpired owner-created Grant is also required and rechecked on every request.
- `system.*` remains bounded by declared operations.
- Disable, stop, remove, trust change, rollback, owner rotation, restart, reprovisioning, expiry, and final Grant revocation fail closed or rotate/revoke the plugin credential.
- Existing signature, timestamp, nonce/replay, explicit-confirmation, idempotency, expected-version, redacted-error, and audit paths remain in use.
- The per-plugin HMAC session key is intentionally delivered only to that supervised process; the owner key is not delivered. This does not claim an OS sandbox, consistent with the accepted Alpha limitation.

## Resulting authorization boundary

Platform accepts Core `322503845cb322628fe2db7a2c570fc6289d22db` as the verified T5 compatibility fix. Official UI U4 may resume against this exact commit. T5 end-to-end installation, lifecycle, recovery, compatibility, resource acceptance, and release-candidate review remain incomplete.
