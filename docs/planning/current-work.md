# Capability Bus 当前工作与 Agent 交接台账

更新时间：2026-10-02（Asia/Shanghai）  
台账状态：`current`  
机器状态来源：[`projects.yaml`](../../projects.yaml)  
中断恢复浮标：[`work-buoy.yaml`](work-buoy.yaml)
长期路线来源：[`master-development-plan.md`](master-development-plan.md)

本文件记录“现在正在做什么、下一步是什么、哪些工作可由其他 Agent 接手”。它不替代 Git 提交、测试输出、`projects.yaml` 或人工门禁报告；任何 Agent 开工前都必须重新核验实际仓库状态。

需要在工作突然终止后接手时，先读取 `work-buoy.yaml` 获取最短恢复路径，再用本台账确认完整边界和后续队列。

## 当前快照

| 项目 | 权威提交 | 工作区 | 当前阶段 |
|---|---|---|---|
| Platform | `a6ef9e4` 停止交接点；本次登记提交 | 提交后应干净 | 新 Platform 对话待创建 |
| Core | `78127bb9192aa6ddf8f15b544c553c149415c5a5` | 13 个已跟踪修改 + 6 组未跟踪内容，全部原样保留 | 新 T3 对话已待命 |
| Official UI | `562828ebb7a760066844c172c23b3b2200abae5a` | 已独立核验干净 | 新 U2 对话已待命；U1 已验证 |
| 来源 personal-ai-control-plane | `7f82511837adf06eef87dd9059b57d59bcaeadd6` | 原有 14 项未提交状态保留 | 只读固定来源 |

新 Core 对话：`01a0fba0-c91d-7981-9633-dbb8bd925df7`。新 Official UI 对话：`01a0fba0-e14d-7610-ac58-10f172ee78e0`。二者均使用 GPT-5.6 Sol / high，已完成只读状态核验并保持待命，只有新 Platform 对话可以激活。旧对话仅保留审计记录。

## 当前可执行事项

| 工作项 | 状态 | 执行者 | 入口/证据 |
|---|---|---|---|
| `CORE-T3` | `new_conversation_standby` | 新 Core 对话 `01a0fba0-c91d-7981-9633-dbb8bd925df7` | 新 Platform 激活消息 |
| `UI-U1` | `verified` | UI 对话 `01a0fa86-b191-70a2-80af-3853312f2286` | UI `562828ebb7a760066844c172c23b3b2200abae5a` |
| `UI-U2` | `new_conversation_standby` | 新 UI 对话 `01a0fba0-e14d-7610-ac58-10f172ee78e0` | 新 Platform 激活消息 |
| `PLATFORM-HANDOFF` | `dispatch_pending` | 新 Platform 对话待创建 | 本停止交接提交 |

用户已回复 `接受 F1`。用户随后要求停止旧项目对话并为 Platform、Core、UI 全部创建新对话重新调度。旧 Core/UI 对话已明确停止；新对话在登记进台账前不得写入。

恢复后首个执行线报告：Core 的 6 项常驻运行故障测试通过，正在覆盖本地 daemon 和独立持久任务/事件插件。UI U1 已提交并由 Platform 在全新临时副本中独立验证：离线重建 19 个依赖、锁文件 SHA-256 `3020486928dd8f424bf2d7d884f431b3c6915fced3fc7d55f68a4f8ead608a89`、27 个 F1 契约逐字节一致、15/15 测试、Vite 构建、19 个许可证声明和 30/30 插件包文件完整性均通过；Core F1 Manifest 校验器接受 `ui.official.web`。U2 已满足自动授权条件。

## 已完成且已核验

| 阶段 | 结果 | 权威证据 |
|---|---|---|
| T0 来源快照与复用审计 | 人工接受 | [T0 报告](../reviews/human-checkpoints/T0-source-reuse-audit-human-review.md) |
| 来源权利检查点 | 人工确认 | [来源权利报告](../reviews/human-checkpoints/T1-source-rights-confirmation-human-review.md) |
| T1 抽取与去产品化 | Platform 验证 | [T1 报告](../reviews/human-checkpoints/T1-core-extraction-human-review.md) |
| T2 alpha.1 最小闭环 | Platform 验证 | [F1 报告](../reviews/human-checkpoints/F1-contract-freeze-human-review.md) |

T2 最终独立复核：源码 71/71、隔离安装 71/71、专项 27/27；18/18 来源基线文件哈希不变。初始 F1 候选中的跨 Provider Grant 漏洞已复现、修正并再次独立验证。

## F1 接受后的待办队列

F1 已接受。Platform 可在独立所属项目对话中并行调度 Core 与 UI；每个 Agent 只能写入其所属仓库。

| ID | 所属项目 | 状态 | 工作范围 | 退出门槛 |
|---|---|---|---|---|
| `CORE-T3` | capability-bus-core | `stopped_dirty_preserved` | alpha.2 可靠运行：恢复、超时、失联、资源治理，以及按架构边界实现的持久任务/事件系统插件能力 | alpha.2 测试和恢复证据通过 |
| `UI-U1` | capability-bus-official-ui | `verified` | 导入冻结契约和 fake Core；建立 UI 系统插件壳、Manifest、协议客户端与不兼容/离线/无权限处理 | 已通过，提交 `562828e` |
| `UI-U2` | capability-bus-official-ui | `stopped_dispatch_pending` | Dashboard、Plugins、Capabilities、Runtime、Audit 只读界面 | mock 驱动验收与无私有状态访问证明 |
| `CORE-T4` | capability-bus-core | `blocked_by_CORE-T3` | alpha.3 身份、安全、配置、SecretRef、兼容、替换与回滚 | 完整初版 Core 与安全测试通过 |
| `UI-U3` | capability-bus-official-ui | `blocked_by_UI-U2_and_Core_contracts` | 受控写操作、确认、结果和审计关联 | 写操作契约与权限测试通过 |
| `F2-HUMAN-ACCEPTANCE` | Platform | `blocked_by_CORE-T4_and_UI-U3` | 完整 Alpha API 与集成兼容矩阵人工冻结 | 人工接受 F2 |
| `T5-INTEGRATION` | Core + UI | `blocked_by_F2` | alpha.4 安装、联调、启停、卸载、恢复、兼容与资源验收 | 候选发布证据齐备 |
| `RELEASE-CANDIDATE-HUMAN-ACCEPTANCE` | Platform | `blocked_by_T5` | 发布候选人工复核 | 人工接受发布候选 |

## 可并行协作边界

F1 接受后可同时安排两个 Agent：

1. Core Agent 只执行 `CORE-T3`，写入 `capability-bus-core`。
2. UI Agent 先执行 `UI-U1`，写入 `capability-bus-official-ui`，只能使用 F1 冻结契约和 fake Core。

Platform Agent 只做调度、核验和状态记录，不写运行时代码。任何 Agent 不得：

- 在 F1 接受前开始上述实现；
- 跨仓库写入或把 Platform 变成第三个实现仓库；
- 把 RSS、Hermes、Review、SiYuan 或固定产品流程放入裸 Core；
- 把对话叙述当作提交/测试证明；
- 越过 F2 或发布候选人工检查点；
- 覆盖来源仓库或其他仓库中的用户修改。

## Agent 接手检查表

接手 Agent 必须按顺序执行：

1. 阅读根 `AGENTS.md`、本台账、`projects.yaml`、总体路线和当前门禁报告。
2. 核验目标仓库的分支、HEAD、工作区和所属项目指令；若与本台账不一致，停止并报告陈旧状态。
3. 确认工作项状态为可执行，并在 Platform 台账中登记负责对话/Agent、基线提交和开始时间。
4. 只在所属仓库实现；记录提交、实际测试命令和结果。
5. 交付后由 Platform 独立复核，再更新本台账和 `projects.yaml`。

## 台账维护规则

Platform 必须在以下事件后更新并提交本文件：授权、派发、完成、验证失败、修正派发、验证通过、进入人工门禁、人工接受或路线/责任边界变化。每次更新至少记录：最新权威提交、工作区状态、活动工作项、负责对话/Agent、测试证据、阻塞项和下一步。

`work-buoy.yaml` 的更新频率更高：派发前、重要提交后、测试里程碑后、进入长时间等待前、发出修正要求后以及任何门禁变化时都要刷新。若发生无法提前记录的硬中断，接手 Agent 必须把浮标视为“最后已知安全点”，并以实际 Git HEAD、工作区和线程记录重建浮标，不能假定浮标之后的步骤已完成。
