# Capability Bus 当前工作与 Agent 交接台账

更新时间：2026-10-03（Asia/Shanghai）
台账状态：`current`  
机器状态来源：[`projects.yaml`](../../projects.yaml)  
中断恢复浮标：[`work-buoy.yaml`](work-buoy.yaml)
长期路线来源：[`master-development-plan.md`](master-development-plan.md)

本文件记录“现在正在做什么、下一步是什么、哪些工作可由其他 Agent 接手”。它不替代 Git 提交、测试输出、`projects.yaml` 或人工门禁报告；任何 Agent 开工前都必须重新核验实际仓库状态。

需要在工作突然终止后接手时，先读取 `work-buoy.yaml` 获取最短恢复路径，再用本台账确认完整边界和后续队列。

## 当前快照

| 项目 | 权威提交 | 工作区 | 当前阶段 |
|---|---|---|---|
| Platform | 本记录所在提交 | 已核验干净 | 仅本地发布准备已完成，禁止公开上传 |
| Core | `c64bc3fe36b7abb1cf33789bf8beee73e9bb4722` | 已核验干净 | 本地制品/校验/元数据已验证；本地 `v0.1.0-alpha.4` 未推送 |
| Official UI | `d77a9a92f7abd435c3d5447db8179e4325f3e79d` | 已核验干净 | 本地制品/校验/元数据已验证；本地 `v0.1.0-alpha.1` 未推送 |
| 来源 personal-ai-control-plane | `7f82511837adf06eef87dd9059b57d59bcaeadd6` | 3 个已跟踪修改 + 18 个未跟踪项，全部保持只读且不作复用来源 | 只读固定来源 |

新 Core 对话：`01a0fba0-c91d-7981-9633-dbb8bd925df7`。新 Official UI 对话：`01a0fba0-e14d-7610-ac58-10f172ee78e0`。二者已完成只读状态核验；后续实现回合统一使用 medium 推理，且同一时间只激活一个实现通道。旧对话仅保留审计记录。

新 Platform 对话：`01a0fba2-3f09-7763-ada8-6e3e0b6b06d1`，使用 GPT-5.6 Sol / high。它必须读取本最终交接提交并重新核验三仓状态，然后激活上述两条待命执行线。本旧 Platform 对话在交接后停止。

## 当前可执行事项

| 工作项 | 状态 | 执行者 | 入口/证据 |
|---|---|---|---|
| `CORE-T3` | `verified` | 新 Core 对话 `01a0fba0-c91d-7981-9633-dbb8bd925df7` | Core `d3556226ec6e8221781e4267ca2aa0b6ca523ebd`；Platform 源树/隔离安装各 92/92 |
| `UI-U1` | `verified` | UI 对话 `01a0fa86-b191-70a2-80af-3853312f2286` | UI `562828ebb7a760066844c172c23b3b2200abae5a` |
| `UI-U2` | `verified` | 新 UI 对话 `01a0fba0-e14d-7610-ac58-10f172ee78e0` | UI `f17320cf39f18d507a4e40b0f7047b88a14582ae`；Platform 全新副本离线门禁通过 |
| `PLATFORM-HANDOFF` | `verified` | 新 Platform 对话 `01a0fba2-3f09-7763-ada8-6e3e0b6b06d1` | Platform `85461db7b56645a881854c872473d15e99ba7b9e`，工作区核验干净 |
| `CORE-T4` | `verified` | Core 对话 `01a0fba0-c91d-7981-9633-dbb8bd925df7` | `272f632`；安装态 100/100、无跳过、编译、F1 零差异、逐操作契约与安全门禁通过 |
| `UI-U3` | `verified` | UI 对话 `01a0fba0-e14d-7610-ac58-10f172ee78e0` | `39b5904`；12 文件契约一致、26/26、构建、边界及 31/31 包完整性通过 |

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

## F2 接受后的待办队列

F1 与 F2 均已接受。Platform 只在独立所属项目对话中按单通道顺序调度 Core 或 UI；每个 Agent 只能写入其所属仓库。

| ID | 所属项目 | 状态 | 工作范围 | 退出门槛 |
|---|---|---|---|---|
| `CORE-T3` | capability-bus-core | `verified` | alpha.2 可靠运行：恢复、超时、失联、资源治理，以及按架构边界实现的持久任务/事件系统插件能力 | 已满足；见 [T3 报告](../reviews/human-checkpoints/T3-core-alpha2-human-review.md) |
| `UI-U1` | capability-bus-official-ui | `verified` | 导入冻结契约和 fake Core；建立 UI 系统插件壳、Manifest、协议客户端与不兼容/离线/无权限处理 | 已通过，提交 `562828e` |
| `UI-U2` | capability-bus-official-ui | `verified` | Dashboard、Plugins、Capabilities、Runtime、Audit 只读界面 | 已满足；见 [U2 报告](../reviews/human-checkpoints/U2-read-only-console-human-review.md) |
| `CORE-T4` | capability-bus-core | `verified` | alpha.3 身份、安全、配置、SecretRef、兼容、替换、回滚及完整公共管理能力 | 已满足；见 [T4 报告](../reviews/human-checkpoints/T4-core-alpha3-human-review.md) |
| `UI-U3` | capability-bus-official-ui | `verified` | 受控写操作、确认、结果和审计关联 | 已满足；见 [U3 报告](../reviews/human-checkpoints/U3-controlled-writes-human-review.md) |
| `F2-HUMAN-ACCEPTANCE` | Platform | `accepted` | 完整 Alpha API 与集成兼容矩阵人工冻结 | 用户回复 `接受F2`；见 [F2 报告](../reviews/human-checkpoints/F2-core-alpha-api-freeze-human-review.md) |
| `T5-INTEGRATION` | Core + UI | `accepted_as_release_candidate` | alpha.4 安装、联调、启停、卸载、恢复、兼容与资源验收 | 已满足；精确候选见 T5 人审报告 |
| `RELEASE-CANDIDATE-HUMAN-ACCEPTANCE` | Platform | `accepted` | 发布候选人工复核 | 用户已回复 `接受发布候选` |
| `CORE-LOCAL-RELEASE-PREP` | capability-bus-core | `verified` | 在精确已接受提交上生成本地制品、校验和本地标签 | `c64bc3f` 制品/校验/元数据及本地 `v0.1.0-alpha.4` 已验证，均未上传/推送 |
| `UI-LOCAL-RELEASE-PREP` | capability-bus-official-ui | `verified` | 对精确 UI `d77a9a9` 构建本地插件包、源码归档、校验/元数据与本地标签 | 28/28、31/31 及本地 `v0.1.0-alpha.1` 已验证，均未上传/推送 |
| `LOCAL-RELEASE-BUNDLE` | capability-bus-platform | `verified` | 组装已忽略的本地 Core/UI 联合包和根校验清单 | `.release-local/capability-bus-alpha4-local-2026-10-03/`；根清单 SHA-256 `add4e925…`；`public_upload=false` |

## 当前协作边界

用户已接受 Core `272f632` 与 UI `39b5904` 组成的 F2 冻结基线。Platform 已独立接受 Core 修复 `3225038`：全新归档、隔离 wheel 安装、102/102 完整测试且无跳过、F1/F2 机器契约零差异、编译与 27 文件包边界通过；额外的真实受监督插件测试证明管理 Socket 调用成功且 Grant 撤销后旧凭据立即失效。证据见 [T5 Core 管理引导报告](../reviews/human-checkpoints/T5-core-management-bootstrap-human-review.md)。UI U4 可以恢复，下一强制人工检查点仍是发布候选接受。

UI U4 恢复回合先通过 28/28 专项与桥接安全测试，再修正真实联调中的 expected-version 测试数据，最终提交 `28f2428`。该提交包含真实管理桥接、回环 HTTP、一次性配对、短期 HttpOnly Cookie、Origin/CSRF 与安全响应头，并保留 fake transport 用于确定性测试。

Platform 已从全新归档独立验证该提交：离线安装 20 个锁定依赖且锁文件哈希不变，F1 与管理契约目录均与 Core `3225038` 零差异，28/28 测试无跳过，构建、20 个许可证、17 项源码边界及 31/31 插件包完整性全部通过；真实 Core 验证覆盖安装、授权、启停、读写、确认、版本与幂等冲突、审计、Grant 撤销、移除、双端重启和不兼容拒绝。证据见 [T5 UI 真实 Core 联调报告](../reviews/human-checkpoints/T5-ui-real-core-integration-human-review.md)。U4 验收完成时没有遗留实现通道；后续只盘点并补齐 T5 十项联合验收中尚缺的发布证据。

T5 十项证据盘点见 [联合验收审计](../reviews/T5-joint-acceptance-audit.md)：6 项已完整覆盖，CLI-only、禁用 UI 后非 UI 工作延续、崩溃/旧协议三类需要更直接的精确候选证据，目标机资源测量尚缺，Core/UI 发布生命周期文档尚未合并。`CORE-T5-release-evidence` 已从 Platform `f52b97a` 派发到既有 Core 对话，范围限定为 CLI、旧协议、崩溃恢复、真实 M4/16 GB/256 GB 测量和 Core 生命周期说明；不得增加功能或改变冻结契约。UI 保持待命，不得并行启动。

Core 随后提交 `c4fe126`，Platform 从全新归档独立验证其 27 文件 wheel、104/104 安装态测试零跳过、编译、冻结契约零变化、CLI-only 闭环、旧协议/不兼容、强制崩溃恢复以及真实目标机 30/30 资源测量；证据见 [T5 Core 发布证据报告](../reviews/human-checkpoints/T5-core-release-evidence-human-review.md)。`UI-T5-release-evidence` 已从 Platform `669dd43` 派发，且是当前唯一活动通道：只补“禁用/移除 UI 后非 UI 工作延续”的联合证明和 UI 生命周期运行手册。

UI 提交 `c3a641f` 后，Platform 在全新归档中再次通过 28/28、24 模块构建、20 许可证、17 边界检查、31/31 包完整性和真实 Core 联调；同一非 UI 驻留进程跨 UI 禁用保持不变，并在 UI 移除后继续健康可调用。证据见 [T5 UI 发布证据报告](../reviews/human-checkpoints/T5-ui-release-evidence-human-review.md)。十项功能验收均已闭合，但 Core 仍打包/自报 `0.1.0-alpha.3`，未满足路线规定的集成节点 `0.1.0-alpha.4`；因此只授权一个不改变协议的版本标记修正通道。

Core 以 `4858520` 完成版本标记修正。Platform 全新归档验证 `0.1.0a4` wheel、105/105 安装态测试零跳过、27 文件边界和完整目标机验收；能力协议仍为 alpha.1、管理协议仍为 alpha.3，两棵冻结契约零变化。现在只允许 UI 把真实联调的精确 Core 提交从 `c4fe126` 刷新为 `4858520` 并重跑既有门禁，之后进入发布候选人工检查点。

UI 最终提交 `d77a9a9` 只刷新精确 Core 固定值和证据文字。Platform 全新归档再次完成离线依赖、28/28、24 模块构建、20 许可证、17 边界、31/31 包完整性以及对 Core `4858520` 的真实联合联调，全部通过。T5 已完成并进入 [发布候选人工检查点](../reviews/human-checkpoints/T5-release-candidate-human-review.md)；当前没有活动实现通道，必须等待用户明确接受或退回。

用户随后明确回复 `接受发布候选`。Core `4858520` 与 UI `d77a9a9` 已登记为正式 T5 / alpha.4 发布候选；当前没有活动实现通道，也没有公开上传、部署或生产凭据操作。后续发布、部署或新阶段必须由新的用户请求启动。

用户现已授权“继续发布准备，但不要公开上传”。Platform 已启动单通道 `CORE-LOCAL-RELEASE-PREP`：只允许对精确 Core 候选 `4858520` 构建本地制品、生成校验/元数据并创建未推送的本地标签。禁止 `git push`、包仓库上传、Release 创建、部署、凭据使用和公网暴露；UI 通道保持待命。

Core 首次本地准备构建已产生临时 wheel/源码归档，但完整安装态套件中强制崩溃恢复用例失败（104/105 通过，2 个沙箱 socket 跳过）。因此这两个临时制品不得视为通过的发布制品，也未生成 `SHA256SUMS`、发布元数据或 `v0.1.0-alpha.4` 标签。当前唯一通道收窄为该用例的定点复现与诊断；禁止公开上传的边界不变。

诊断证明运行时已换新实例，但测试在监控线程将状态从 `degraded` 提交为 `healthy` 前提前调用；Core 只修正了发布证据测试的等待条件，提交 `c64bc3f`。Platform 已独立审查该单测试文件差异，候选验证脚本通过，从新 wheel 隔离安装的完整套件 105/105 通过（2 个沙箱 socket 跳过）。该精确提交现为 Core 本地制品新基线。

Core 终态本地制品已产生并由 Platform 复核：wheel `21df5c30…`、源码归档 `c1032dd0…`、`SHA256SUMS` 全部通过，元数据明确 `public_upload=false`；annotated tag 对象 `b0f3e719…` 解引到 `c64bc3f…`。任何内容均未上传或推送。单通道现切换到 `UI-LOCAL-RELEASE-PREP`。

UI 本地准备随后通过 28/28 测试、构建/许可证/源码边界与 31/31 包完整性检查；本地 tag 对象 `4956be04…` 解引到 `d77a9a9…`。Platform 已把两个项目的制品、元数据和校验清单组装到忽略目录 `.release-local/capability-bus-alpha4-local-2026-10-03/`，根 `BUNDLE-SHA256SUMS` 全部通过，其自身 SHA-256 为 `add4e925b05b427f385b63f84414c53435ea2ec8bae42a4f94d50665afd191f1`。本地准备到此完成；不存在活动通道，也没有任何公开上传、标签推送、远程 Release 或部署。详见 [本地发布准备记录](../reviews/T5-local-release-preparation.md)。

三个注册对话的额度与效率根因见 [2026-10-03 使用效率审计](../reviews/usage-efficiency-audit-2026-10-03.md)。后续调度强制遵循 [低消耗执行策略](../governance/usage-efficiency-policy.md)：单实现线、协调低推理、实现中推理、每回合 12 次模型/工具往返软上限，以及收敛后的单次完整测试与验收。

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
