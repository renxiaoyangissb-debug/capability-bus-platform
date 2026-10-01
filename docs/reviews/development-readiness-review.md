# Development readiness review

Status: **PLATFORM WORKFLOW AUTHORITY ACTIVE — WAITING FOR SOURCE-RIGHTS CONFIRMATION**

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
- [ ] Confirm source ownership and permission to reuse, modify, and distribute committed source code before T1 extraction.
- [ ] Accept F1 before UI business implementation.
- [ ] Accept F2 before concentrated alpha.4 integration.

## Approval record

The user approved preparation against Platform commit `ddcfa0974637bbdd9a91d93b87f0a7f983eda8af` on 2026-10-01 (Asia/Shanghai), accepted T0, and subsequently delegated in-scope workflow authorization to Platform. The active authority is recorded in [Platform delegated authority](../governance/platform-delegated-authority.md). Formal source-code extraction remains disabled until the source-rights checkpoint is resolved.

## T0 evidence review

Core delivered T0 under commit `aa6269842fa034c8849c579b0262dff4db7ee8cc`. Platform independently verified the commit scope, 36 source blob/SHA-256 rows, actual offline test execution evidence, and preservation of source/UI state. The user then replied “符合通过”, recorded as acceptance of the T0 review published in Platform commit `68f9704bec3b957306258c6f18b091972b7c76f7`. See [the human review report](human-checkpoints/T0-source-reuse-audit-human-review.md) for the evidence and remaining pre-T1 decision. T0 acceptance and delegated workflow authority do not establish the missing reuse/distribution permission basis.

## First authorized action

The first authorized T0 action has been completed and accepted. After the source-rights checkpoint is resolved, Platform may authorize and dispatch T1 automatically under the approved reuse matrix. No separate per-task authorization is needed unless another mandatory checkpoint applies.
