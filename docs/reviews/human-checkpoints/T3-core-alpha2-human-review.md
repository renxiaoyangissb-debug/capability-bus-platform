# Core T3 / Alpha.2 可靠运行复核报告

状态：`platform_verified`；后续阶段：`T4_not_started_by_current_dispatch_scope`

核验日期：2026-10-02（Asia/Shanghai）

本报告保留 Core T3 的独立复核证据。T3 不是强制人工接受门；Platform 根据已委托权限完成核验。本次重新调度明确只完成 T3，不开始 T4 或 F2，因此本报告不自动派发后续 Core 工作。

## 提交与工作区

| 项目 | 已核验状态 |
|---|---|
| Core F1/T3 起点 | `78127bb9192aa6ddf8f15b544c553c149415c5a5` |
| Core T3 交付 | `d3556226ec6e8221781e4267ca2aa0b6ca523ebd` |
| Core 工作区 | 干净，`main` |
| Platform 派发前检查点 | `9f3d16f3ec4ec692fb1e278fe17bec64284307a6` |
| Platform UI 候选拒收检查点 | `53a6470a02db0719de2063e0c1f1c71aa895586d` |

交付包含 resident 进程监督、崩溃恢复、超时与取消、幂等副作用对账、资源治理、本地 Unix socket 控制通道，以及独立 `system.durable-reliability` 系统插件中的持久 Task/Event/checkpoint/lease/retry/backoff/dead-letter 能力。

## Platform 独立验证

Platform 在 2026-10-02 使用临时安装、pycache 与 wheel 目录离线复核，未使用网络、真实凭据或生产数据：

| 验证项 | 结果 |
|---|---|
| Core 源树全量测试 | 92/92 通过，无跳过 |
| 全新 `--target` 隔离安装 | 成功，版本 `0.1.0-alpha.2` |
| 从隔离安装目标重跑全量测试 | 92/92 通过，无跳过 |
| `src`、`tests`、`system_plugins` 编译检查 | 通过 |
| F1 `contracts/v0.1/` 相对接受提交 | 零差异 |
| `vendor/source-baseline/` 相对抽取提交 | 零差异 |
| 独立 wheel | `capability_bus_core-0.1.0a2-py3-none-any.whl`，SHA-256 `aab80116edbd1be4105bc5de43ac799c4b8764fa6875aae170aa06004fb881a7` |
| wheel 内容 | 仅 Core、SDK、分发元数据与 `capbus` 入口；不含系统插件、测试、示例、契约或来源基线 |

Core 自报构建与 Platform 独立构建的 wheel 哈希不同；当前阶段未声明 bit-for-bit 可复现 wheel，Platform 以不可变 Git 提交、隔离安装、文件边界和两次 92 项测试为验收依据。发布候选阶段仍需形成固定发布产物证据。

## 架构与安全边界

- Core SQLite 只增加调用恢复、幂等对账、运行时所有权与资源测量元数据，不含 Task/Event 表。
- 持久 Task/Event 能力位于独立进程系统插件，使用插件自己的数据目录和 SQLite，不导入 Core 或 SDK。
- 授权仍先于幂等查询；逻辑操作身份仍与 Provider 无关。
- 过期或可能越过进程边界的副作用进入显式 reconciliation，不盲目重放。
- 本地控制通道仅使用权限 `0600` 的 Unix socket，不监听 TCP；重复 daemon 不接管现有 resident 进程。
- 未声明 OS 级文件系统、网络、CPU 或内存沙箱；完整 Trust/Grant、配置历史、Secret Store 与公开管理写契约仍属于后续阶段。
- F1 冻结契约未修改，未开始 T4/F2，未引入 UI、MCP 或产品适配器。

## 阶段判定

- [x] T3 可靠运行与恢复证据通过。
- [x] 持久 Task/Event 能力保持独立系统插件边界。
- [x] F1 合同与来源基线保持不变。
- [x] Core 安装包保持裸 Core 边界。
- [x] Core 工作区干净，提交可验证。
- [ ] T4 未开始；本次调度不授权自动进入 T4。
- [ ] F2 未开始，仍是下一项强制人工接受门。

## 当前决定

Platform 接受 `d3556226ec6e8221781e4267ca2aa0b6ca523ebd` 作为已验证的 Core T3 / `0.1.0-alpha.2` 交付。Core 对话停止在 T3 完成点；在新的明确调度前不得开始 T4 或宣称 F2。
