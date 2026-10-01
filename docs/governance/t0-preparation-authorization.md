# Core T0 preparation authorization

Recorded on 2026-10-01 (Asia/Shanghai) from the user's explicit message in the Platform coordination conversation `01a0f793-d764-7ae0-bf1b-377fbe764bb7`.

Approved Platform baseline: `ddcfa0974637bbdd9a91d93b87f0a7f983eda8af`.

## User approval (verbatim)

```text
APPROVE CAPABILITY BUS ALPHA DEVELOPMENT PREPARATION UNDER PLATFORM COMMIT ddcfa09.

批准总体开发方案和当前架构基线。

授权范围：

1. 启动 capability-bus-core 的 T0 来源快照与复用审计；
2. 允许创建并调度 Core 项目开发对话；
3. 读取 personal-ai-control-plane，记录来源分支、HEAD、工作区状态、文件哈希和测试证据；
4. 生成 source-snapshot、reuse-matrix、provenance 和必要 ADR；
5. UI 项目只保持 U0 设计准备，不得开始业务实现；
6. T0 完成后停止，提交人工复核报告。

本次不授权 T1/T2 功能实现，不授权越过 F1，不授权 UI 功能开发。保留所有用户现有修改。
```

## Authorized execution and evidence

- Core T0 conversation: `01a0f7c7-8a3b-78d2-adaa-8f1f83c3f34e`, local Core project.
- Core initial baseline: `d84c495acbb69f588a5412f618952950fd4e5126`.
- UI protected baseline: `7365453781aecdf7909d8d46282c0d09e8050c3a` (clean at dispatch).
- Source initial observed HEAD: `7f82511837adf06eef87dd9059b57d59bcaeadd6`, branch `main`, dirty documentation worktree. Core must independently record the source snapshot and preserve those changes.
- Deliverables: source-snapshot, reuse-matrix, provenance, necessary boundary ADRs, reproducible offline source test evidence, and a verifiable Core documentation commit.
- Only Core owns T0 deliverable writes; the source and UI remain read-only for this execution.
- Platform verifies the returned Git commit and file/test evidence before issuing the human review report.

## Unapproved scope and stopping point

No T1 extraction baseline, T2 runtime implementation, F1/F2 acceptance, UI business implementation, or UI task dispatch is authorized. T0 completion does not authorize continuation. After verification, the program returns to `waiting_for_human_review` with T0 review pending and formal implementation authorization still false.

## Execution result

T0 was delivered in Core commit `9255224d7038ef569121d0bea3e2e2e1189b841f`, followed by documentation count correction `aa6269842fa034c8849c579b0262dff4db7ee8cc`. The Core conversation is idle and stopped. Platform verified the evidence and returned to `waiting_for_human_review`. See [the T0 human review report](../reviews/human-checkpoints/T0-source-reuse-audit-human-review.md). T1/T2 and UI business implementation remained unauthorized under this historical T0 authorization; later workflow authority is governed by `platform-delegated-authority.md`.

## Subsequent human acceptance

After the T0 report was published in Platform commit `68f9704bec3b957306258c6f18b091972b7c76f7`, the user replied verbatim: “符合通过”. This accepts the T0 audit delivery at Core commit `aa6269842fa034c8849c579b0262dff4db7ee8cc`. The later delegated-authority policy controls future routine workflow authorization. The reply does not supply source ownership/licensing terms; that remains a mandatory human checkpoint before T1 source extraction.
