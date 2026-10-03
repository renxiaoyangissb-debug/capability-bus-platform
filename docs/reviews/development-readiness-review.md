# Development readiness review

Status: **T5 / ALPHA.4 RELEASE CANDIDATE ACCEPTED**

## Prepared

- [x] Core repository exists as an independent Git repository.
- [x] Official UI repository exists as an independent Git repository.
- [x] Core Alpha design baseline exists.
- [x] Official UI Plugin Alpha design baseline exists.
- [x] Overall development route defines Core-first sequencing.
- [x] F1 and F2 freeze gates are defined.
- [x] Existing-code extraction is mandatory and mapped to source modules.
- [x] UI is defined as a registered, removable system plugin.
- [x] Coordination repository has no runtime authority.

## Human decisions required before formal development

- [x] Approve the master development plan without architectural changes.
- [x] Approve starting Core T0 source snapshot and reuse audit.
- [x] Confirm UI remains in U0 preparation until F1.
- [x] Confirm reading the source repository and coordinating Core T0; prior read-only inspection of all three repositories remains in scope.
- [x] Explicitly authorize creating and dispatching the Core T0 conversation only.
- [x] Accept the verified Core T0 audit delivery.
- [x] Delegate routine workflow authorization and Core/UI task dispatch to Platform.
- [x] Confirm source ownership and permission to reuse, modify, and distribute committed source code before T1 extraction.
- [x] Accept F1 before UI business implementation.
- [x] Accept F2 before concentrated alpha.4 integration.
- [x] Accept the exact T5 / alpha.4 release candidate before any release publication.

## Approval record

The user approved preparation against Platform commit `ddcfa0974637bbdd9a91d93b87f0a7f983eda8af` on 2026-10-01 (Asia/Shanghai), accepted T0, and subsequently delegated in-scope workflow authorization to Platform. The active authority is recorded in [Platform delegated authority](../governance/platform-delegated-authority.md). The owner then confirmed authority to reuse, modify, and distribute `personal-ai-control-plane@7f82511837adf06eef87dd9059b57d59bcaeadd6`; formal Core T1 extraction is authorized.

## T0 evidence review

Core delivered T0 under commit `aa6269842fa034c8849c579b0262dff4db7ee8cc`. Platform independently verified the commit scope, 36 source blob/SHA-256 rows, actual offline test execution evidence, and preservation of source/UI state. The user then replied “符合通过”, recorded as acceptance of the T0 review published in Platform commit `68f9704bec3b957306258c6f18b091972b7c76f7`. See [the human review report](human-checkpoints/T0-source-reuse-audit-human-review.md) for the evidence and the pre-T1 decision that remained at that time. The later source-rights confirmation is recorded separately in [the T1 source-rights human review](human-checkpoints/T1-source-rights-confirmation-human-review.md).

## Current authorized action

Core T4 at `272f6325506c4ce7c4de5c100fb3f03a28701318` and Official UI U3 at `39b5904499c0ecfa0dff1d57683812c150e377e4` were independently verified and accepted as F2. T5 then completed under the low-consumption single-lane policy. Platform independently verified Core `4858520224a98b6528cb6cf47e7aa2e517bb62dc` and Official UI `d77a9a92f7abd435c3d5447db8179e4325f3e79d`; the user replied `接受发布候选`, accepting the exact pair and the limitations recorded in the [release-candidate human review](human-checkpoints/T5-release-candidate-human-review.md).
