# Platform delegated workflow authority

Status: active  
Effective date: 2026-10-01 (Asia/Shanghai)

User instruction recorded in the Platform coordination conversation:

```text
设置platform项目拥有授权权限，自动授权工作流直到必须人工介入。
但要保留人工复核报告，报告在指定目录下，按照开发进度命名。
```

## Delegation

The project owner delegates authorization for the approved Capability Bus development workflow to this Platform coordination project. Within the fixed architecture, master development plan, active gate, and repository ownership boundaries, Platform may:

- create, continue, and message Core and Official UI project conversations;
- authorize narrowly scoped owning-project work;
- advance routine phases when prerequisite evidence is verified;
- request corrections, rerun safe verification, and accept routine engineering evidence;
- update coordination status, gate records, compatibility records, and review reports.

This delegation removes the requirement for separate human approval for every task or conversation message. It does not transfer implementation ownership to Platform: Core and UI changes remain in their own repositories and project conversations.

## Mandatory human checkpoints

Platform must stop and request a human decision when:

1. source ownership, reuse, modification, or distribution authority is missing or ambiguous;
2. a proposal changes the fixed architecture or crosses Core/UI authority boundaries;
3. provenance or authoritative source state cannot be verified;
4. a destructive migration, data-loss risk, broad permission expansion, public network exposure, credential use, or irreversible external effect is required;
5. a frozen contract needs a breaking change without an already approved version and migration path;
6. F1 Contract Freeze, F2 Core Alpha API Freeze, or a release candidate is ready for acceptance;
7. conflicting verified evidence cannot be resolved safely within the approved plan.

Questions of product scope, priority, or user-facing behavior that materially change the approved Alpha plan also require human direction.

## Automatic progression

Outside the checkpoints above, Platform may progress automatically:

1. verify the current repository HEAD, worktree, instructions, and prerequisite gate;
2. write a narrow task for the owning project;
3. dispatch or continue the owning project conversation;
4. wait for completion and independently verify commits, tests, provenance, and scope;
5. require corrections when evidence is incomplete;
6. record the outcome and advance to the next permitted task.

Failure of one task does not permit scope expansion. Platform may retry or request an in-scope fix, but it must stop when a mandatory checkpoint is reached.

## Freeze and UI rules

- F1 and F2 remain hard gates.
- UI stays in U0 until F1 evidence is complete and the human accepts F1.
- F2 must be accepted by the human before concentrated alpha.4 integration.
- Release-candidate publication requires human acceptance.
- A report or narrative does not satisfy a gate without verified commits and test evidence.

## Human-review reports

All reports prepared for human checkpoints live under `docs/reviews/human-checkpoints/`. File names follow:

```text
<progress-id>-<short-subject>-human-review.md
```

Examples:

- `T0-source-reuse-audit-human-review.md`
- `T1-core-extraction-human-review.md`
- `F1-contract-freeze-human-review.md`
- `F2-core-alpha-api-freeze-human-review.md`
- `T5-release-candidate-human-review.md`

Reports are append-only evidence records after acceptance. Later corrections must preserve the accepted commit and explain the correction.

## Current checkpoint

T0 is accepted and the source-rights checkpoint is resolved. Platform independently verified Core T1 at `cf7e87d814ad97d15b100c42c53a606b273b8fd5`; the evidence is recorded in [the T1 Core extraction review](../reviews/human-checkpoints/T1-core-extraction-human-review.md). T2 is authorized for automatic dispatch and supervision. The next mandatory human checkpoint is F1 unless an earlier hard stop is encountered.
