# U3 controlled writes Platform verification

Date: 2026-10-03 (Asia/Shanghai)

## Disposition

Official UI U3 is verified by Platform at:

`39b5904499c0ecfa0dff1d57683812c150e377e4`

The UI remains a contract-fake-driven interface plugin. This verification does not authorize real Core integration or accept F2.

## Contract baseline

- Core authority: `272f6325506c4ce7c4de5c100fb3f03a28701318`
- Management protocol: `0.1.0-alpha.3`
- Imported management snapshot: 12 files
- Provenance records an SHA-256 for every imported file.
- Platform independently compared the UI snapshot with the exact Core directory; no differences were found.

## Independent verification

Platform exported UI commit `39b5904` into a fresh temporary tree and verified:

- offline bootstrap installed 20 lock-selected packages;
- standard `npm ci --offline --ignore-scripts --no-audit --no-fund` succeeded;
- `package-lock.json` SHA-256 remained `c8ef2875aa4c2ae0e7c0a62dd4c9c0081f6527b6ea6194c108f6f5a73bef38e5` throughout;
- 27 F1 files and six reference identities still match the accepted F1 baseline;
- all 12 management contract files match Core `272f632`;
- 26/26 tests passed with no skips;
- TypeScript checking and the 27-module Vite build passed;
- 20 installed dependencies declared licenses;
- 16 source and plugin boundary checks passed;
- packaged plugin integrity verified 31/31 files;
- the owning UI worktree remained clean.

## Accepted interaction boundaries

- The management client and fake are driven by the 42-operation public catalog.
- High-impact writes require explicit confirmation; the UI cannot synthesize confirmation silently.
- Idempotency keys, expected versions, request IDs, trace IDs, and audit IDs are represented.
- Permission, version, idempotency, offline, incompatible, and generic error states remain distinct.
- Secret values are rejected; only SecretRef values cross the interface boundary.
- No Core Python import, SQLite access, private endpoint, real credential, public listener, or real Core integration was added.

## Remaining gate

Platform must present the combined F2 report and stop for human acceptance before T5 integration begins.
