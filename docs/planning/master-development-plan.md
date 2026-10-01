# Capability Bus 两项目总体开发设计与交接方案

状态：Alpha 开发基线  
适用项目：`capability-bus-core`、`capability-bus-official-ui`  
来源项目：`personal-ai-control-plane`  
架构约束：保持已固化的 Capability Bus Core 与 Official UI Plugin 设计不变

## 1. 文档用途

本文是两个新项目的总开发路线、阶段门禁和跨项目交接协议，可直接发送给两个项目各自的 Codex 对话作为开发约束。

本文不替代下列设计基线，而是规定它们的实施顺序：

- 核心设计：`capability-bus-core/docs/design/capability-bus-core-alpha-design-prompt.md`
- UI 设计：`capability-bus-official-ui/docs/design/official-ui-plugin-alpha-design-prompt.md`
- 原开发工作流：`docs/planning/capability-bus-alpha-development-workflow.md`

发生冲突时，以上设计基线的架构边界优先；本文负责路线、冻结点、复用规则和两个项目之间的交付关系。

## 2. 不变的总体架构

### 2.1 核心定位

`capability-bus-core` 是小型、可独立运行、CLI-first 的插件能力总线。核心只保留统一管理不可缺少的机制：

- 插件身份、安装、注册、启停、升级、替换和卸载；
- 版本化 Capability、Manifest、Request/Result/Event Envelope；
- 路由、授权校验、幂等、任务状态、事件和审计；
- Workspace、ContentRef、SecretRef、网络权限和资源约束；
- 稳定系统 ID、权威状态所有权和协议兼容检查。

产品行为、Agent、Skill、工作流、知识库、审批策略、图形界面和具体工具均不进入裸核心。

### 2.2 UI 定位

`capability-bus-official-ui` 是官方维护的系统插件，不是核心内置模块。它必须：

- 通过与第三方插件相同的包格式和 Manifest 注册；
- 经 CLI 安装、授权和启用后才提供界面；
- 只调用公开 Capability，不访问核心数据库或私有 API；
- 可被禁用、卸载和替换；
- 停止后不影响 CLI 和核心任务执行。

### 2.3 禁止事项

- 不得把旧项目固定的 RSS → Hermes → Review → SiYuan 流程带入核心；
- 不得让插件互相 import、访问彼此数据库或形成第二套核心状态；
- 不得因为开发 UI 而给核心增加 UI 专属私有接口；
- 不得以进程内 `importlib` 作为不受信第三方插件的默认安全边界；
- 不得用重新编写替代可以验证抽取的既有通用实现。

## 3. 总开发路线

```text
T0 旧项目冻结快照与复用审计
  ↓
T1 核心代码抽取、去产品化与工程骨架
  ↓
T2 协议草案 + 核心 0.1.0-alpha.1 可执行闭环
  ↓
F1 Contract Freeze 1（UI 实现启动门）
  ├──────────────→ UI-U1 工程骨架与协议客户端
  ↓                         ↓
T3 核心 alpha.2 可靠运行    UI-U2 只读管理界面
  ↓                         ↓
T4 核心 alpha.3 完整初版    UI-U3 受控写操作
  ↓                         ↓
F2 Core Alpha API Freeze / UI Integration Freeze
  └──────────────┬──────────┘
                 ↓
T5 alpha.4 联调、安装包、端到端验收和候选发布
```

开发优先级明确如下：

1. **先开发核心。** UI 在 F1 前只允许整理需求、信息架构、视觉原型和测试用例，不得假设未冻结接口。
2. **核心达到 alpha.1 且通过可执行契约测试后执行 F1。** 从此启动 UI 的代码实现。
3. **F1 后前后端并行。** 核心继续完成可靠性和安全能力；UI 严格基于冻结契约开发。
4. **核心达到 alpha.3 后执行 F2。** 此时形成完整初版核心，冻结 Alpha 集成 API，进入集中联调。
5. **alpha.4 是联合版本。** 只有裸核心、UI 插件安装/启停、权限、恢复和卸载验证全部通过，才可发布候选版本。

## 4. 阶段与门禁

### T0：来源快照与复用审计

由核心项目先执行。开始任何实现前记录：

- 来源仓库 URL/路径、分支、Git HEAD、工作区状态；
- 可复用源文件、测试、Schema 和行为；
- 每项内容的 `extract / adapt / pluginize / retire` 决策；
- 来源文件哈希、目标文件和后续修改原因；
- 旧项目未提交修改不得被默认视为稳定来源。

交付物：

- `docs/extraction/source-snapshot.md`
- `docs/extraction/reuse-matrix.md`
- `docs/extraction/provenance.md`
- `docs/adr/` 中的边界决策

退出条件：每个计划实现的基础能力都已证明“复用、适配或确需新建”，不得直接开始重写。

### T1：核心抽取与去产品化

从来源项目提取通用实现和测试，先保留行为，再重命名和解耦：

1. 建立可回溯的原样抽取提交；
2. 搬迁相应测试并确认抽取前后行为一致；
3. 去除 `personal_ai` 命名和产品固定配置；
4. 将 Hermes、SiYuan、RSS、Review 等实现移出核心，保留为未来插件参考；
5. 将 SQLite 与普通文件保留为 Alpha 默认基础设施；
6. 每次重构保持测试通过，禁止“参考旧代码重新写一套”。

退出条件：裸核心工程可安装，抽取能力有来源记录，产品专属插件不再是核心依赖。

### T2：`0.1.0-alpha.1` 最小闭环

实现并验证：

- Manifest 加载与 Schema 校验；
- 插件安装、注册、启用、禁用、查询、卸载；
- Capability 注册、发现、路由和调用；
- Request/Result/Event Envelope；
- ContentRef 和最小 Workspace；
- 最小授权校验、幂等执行、状态和结构化审计；
- CLI-only 离线演示；
- 一个未知工具插件的注册与调用；
- Provider 替换后稳定系统 ID 和幂等身份不失效。

退出条件：单元测试、契约测试、CLI 演示、重启后状态核验全部通过。

### F1：Contract Freeze 1

F1 是启动 UI 实现的唯一门槛。冻结内容：

- Plugin Manifest v0.1；
- Request/Result/Event Envelope v0.1；
- Capability 命名、发现与调用接口；
- Plugin 生命周期和状态枚举；
- Grant、错误码、审计摘要和分页约定；
- ContentRef、SecretRef 的外部表示；
- UI 所需公开 Capability；
- 契约兼容规则与弃用流程。

冻结不代表永不修改。F1 后的破坏性变更必须：升级协议版本、提供迁移说明、更新契约测试，并由核心和 UI 两项目共同记录。

F1 交付包：

- 版本化 JSON Schema/OpenAPI 或等价机器可读契约；
- 契约示例和错误示例；
- UI 可运行的 mock/fake core；
- 契约一致性测试套件；
- `F1-CONTRACT-FREEZE.md`。

### T3：`0.1.0-alpha.2` 可靠运行

核心继续实现：

- 持久 Task/Event；
- lease、checkpoint、pause/resume；
- retry、退避、dead-letter；
- 幂等与副作用对账；
- 崩溃恢复、超时和插件失联；
- 并发 1 的重型任务默认策略；
- 磁盘配额、缓存清理和资源测量。

同期 UI 只能实现 F1 已冻结能力：

- UI 插件包、Manifest 和启动器；
- 核心协议客户端；
- Dashboard、Plugins、Capabilities、Runtime、Audit 的只读页面；
- mock 驱动测试；
- 核心不可用、版本不兼容和权限不足状态。

### T4：`0.1.0-alpha.3` 完整初版核心

核心补齐：

- 持久稳定身份和 Provider 无损替换；
- Authority、Trust Zone、ApprovalGrant/PromotionProof 基础机制；
- Workspace 隔离、SecretRef、默认禁网和显式网络授权；
- 协议向后兼容、迁移与升级；
- 插件来源、完整性和生命周期治理；
- 权威状态所有权检查；
- 禁止第二套 Core 状态系统；
- 插件移除、替换、恢复和回滚测试。

同期 UI 增加受控写操作：

- 插件安装、启停、升级、卸载；
- Grant 和 Trust Zone 管理；
- 配置编辑与 SecretRef 选择；
- retry、dead-letter 和任务控制；
- 所有高影响操作的预览、确认、结果和审计关联。

退出条件：核心不依赖 UI 可完整运行；UI 不访问核心私有状态；安全与替换测试通过。

### F2：Core Alpha API Freeze

冻结 alpha.4 联调所需的完整公开 API。F2 后只接受缺陷修复和兼容性补充，不再加入新产品功能。

必须产出：

- 核心版本与 UI 最低/最高兼容矩阵；
- 完整 conformance suite；
- 核心和 UI 的安装、升级、降级、卸载步骤；
- Alpha 已知限制与安全说明。

### T5：`0.1.0-alpha.4` 联合验收

联合验证：

1. 全新环境安装裸核心；
2. 仅通过 CLI 运行完整核心闭环；
3. 通过 CLI 安装并授权官方 UI 包；
4. UI 启用后完成插件和任务管理；
5. 禁用 UI 后核心任务继续运行；
6. 卸载 UI 后不残留权威状态；
7. 核心重启、UI 重启、插件崩溃及恢复；
8. 旧协议插件兼容和不兼容插件拒绝；
9. M4/16 GB/256 GB 目标机资源验收；
10. 默认配置无真实凭据、无外网也可安全运行。

## 5. 后端项目路线：`capability-bus-core`

### C0 工程与治理

- 建立 `AGENTS.md`、README、CHANGELOG、版本策略、ADR、测试目录和验证命令；
- 固化小核心、插件隔离、默认禁网、SQLite/普通文件和本机资源约束；
- 建立来源追踪和复用门禁。

### C1 代码抽取

- 抽取 contracts、manifest、registry、router；
- 抽取 state、executor、events；
- 抽取 governance、governor；
- 同步抽取相关测试；
- 将 reference flow 和具体 adapters 留在来源项目，不进入裸核心。

### C2 最小运行面

- 定义新包名和 CLI；
- 完成插件生命周期、Capability 调用和离线示例；
- 建立协议 Schema 和 conformance fixtures；
- 达到 alpha.1，并执行 F1。

### C3 可靠性与恢复

- 形成 alpha.2；
- 完成任务恢复、幂等、事件投递和资源治理；
- 提供 UI 可消费但不为 UI 定制的公开观测能力。

### C4 安全与完整核心

- 形成 alpha.3；
- 完成身份、权限、Workspace、SecretRef、网络、兼容、替换和回滚；
- 执行 F2。

### C5 集成发布

- 与官方 UI 完成 alpha.4；
- 发布核心包、契约包、示例插件、运维文档和验收证据。

## 6. 前端项目路线：`capability-bus-official-ui`

### U0（F1 前）：只做设计准备

- 固化信息架构和页面状态；
- 编写用户流程、权限矩阵、错误/空状态和验收用例；
- 确定资源预算和本地访问安全；
- 不编写依赖猜测接口的业务实现。

### U1（F1 后）：工程与插件壳

- 导入冻结契约和 mock core；
- 建立 UI 插件 Manifest、安装包、启动/停止协议；
- 建立类型生成或契约校验；
- 完成版本不兼容、无权限和核心离线处理。

### U2：只读控制台

- Dashboard；
- Plugins；
- Capabilities；
- Runtime；
- Audit；
- 所有数据只来自公开 Capability。

### U3：受控管理操作

- 插件生命周期；
- 授权与 Trust Zone；
- 配置和 SecretRef；
- 任务控制、重试和 dead-letter；
- 危险操作确认和审计关联。

### U4（F2 后）：真实核心联调

- 替换 mock 为真实核心；
- 运行 conformance suite；
- 验证安装、启停、卸载、恢复和兼容矩阵；
- 与核心共同形成 alpha.4 候选发布。

## 7. 旧项目代码复用清单

以下映射是初始复用要求，执行 T0 时必须逐项核实，不能直接改写成新实现：

| 来源实现 | 已有能力 | 目标处理 |
|---|---|---|
| `src/personal_ai/contracts.py` | ContentRef、请求/结果/事件信封 | 抽取代码和测试，去产品命名，升级为版本化契约 |
| `src/personal_ai/manifest.py`、`contracts/v1/` | Manifest 模型与 Schema | 抽取并补充生命周期、协议兼容和安全字段 |
| `src/personal_ai/registry.py` | 插件发现、装载与注册 | 抽取注册语义；隔离不受信插件，移除默认 importlib 边界 |
| `src/personal_ai/router.py` | Capability 路由、授权和幂等入口 | 抽取行为，改用稳定 Provider-neutral 身份 |
| `src/personal_ai/state.py` | SQLite Task/Event/幂等/lease 状态 | 重点抽取，拆除产品流程字段，保留并发控制和迁移测试 |
| `src/personal_ai/executor.py` | Task 执行、重试与错误归一化 | 抽取，接入通用插件运行边界和副作用对账 |
| `src/personal_ai/events.py` | 事件分发 | 抽取，补充持久投递、重放和 dead-letter |
| `src/personal_ai/governance.py` | CallerContext、AuthorityManager | 抽取机制，演进为 Grant/Trust Zone，策略本身允许插件化 |
| `src/personal_ai/governor.py` | 资源治理 | 抽取并按 M4/16 GB/256 GB 约束扩充配额与互斥任务 |
| `src/personal_ai/plugin.py` | 插件上下文与基础接口 | 抽取接口语义，重构运行边界，避免插件互相 import |
| `src/personal_ai/runtime.py` | 组装运行时 | 只抽取通用装配模式，删除固定产品和 Adapter 依赖 |
| `tests/test_system.py` 等 | 闭环和模块行为证据 | 与实现同步抽取，先证明等价，再扩展新协议测试 |
| `plugins/*` | RSS、Hermes、Review、SiYuan、Inbox 示例 | 不进入核心；作为未来外部插件迁移样本和兼容测试素材 |
| `reference_flow.py`、CLI 固定 demo | 固定产品编排 | 不进入核心运行语义；只提取通用测试方法和演示框架 |

### 强制复用流程

每一项复用必须执行：

1. 记录来源 Git commit、文件路径和 blob/hash；
2. 原样提取实现与对应测试，形成独立 `extraction baseline` 提交；
3. 在新仓库运行测试，记录缺失依赖和行为差异；
4. 再进行包名修改、接口拆分和去产品化；
5. 每次重构保留可比测试；
6. 新写代码必须说明旧实现为何不能复用；
7. 不复制旧项目未提交内容，除非先单独审计并固化来源快照。

“看过旧代码后凭印象重写”不算复用。

## 8. 跨项目契约与协作

### 权威来源

- 核心仓库是协议、Schema、错误码和 Capability 语义的权威来源；
- UI 仓库是交互、视觉、可访问性和 UI 插件打包的权威来源；
- UI 仓库不得长期保存人工维护的核心设计副本；应记录来源版本，或由发布流程同步生成；
- 总路线和兼容矩阵由协调层维护。

### 变更流程

1. 提交核心契约变更提案；
2. 更新机器可读 Schema 和契约测试；
3. 评估 UI 与第三方插件影响；
4. 非破坏性变更进入当前协议小版本；
5. 破坏性变更升级协议并提供迁移路径；
6. 两项目分别记录实现状态，不用口头约定替代文件证据。

## 9. 可直接发送给核心项目对话的任务指令

```text
按照仓库中的 Core Alpha 设计基线和《Capability Bus 两项目总体开发设计与交接方案》继续开发 capability-bus-core。

先读取 AGENTS.md、README、全部设计/规划文档，检查 Git HEAD、工作区和测试状态。架构保持不变：核心是 CLI-first 的最小插件能力总线，产品行为、UI、Agent、工作流和知识库均不得进入裸核心。

从 T0 开始。首先审计 personal-ai-control-plane 的可复用实现，生成 source-snapshot、reuse-matrix 和 provenance。contracts、manifest、registry、router、state、executor、events、governance、governor、plugin、runtime 以及对应测试必须采用“原样抽取基线提交 → 测试 → 去产品化重构”的方式复用，禁止参考后重新编写。固定 RSS/Hermes/Review/SiYuan 流程和具体插件不得进入核心。

依次完成 T0、T1、T2。达到 0.1.0-alpha.1、契约测试和 CLI 离线闭环通过后，生成 F1-CONTRACT-FREEZE.md、机器可读契约、示例、mock/fake core 和 conformance suite。F1 是 UI 开始实现的交接点。未达到硬门禁不得宣称冻结。

保留用户已有修改；默认离线、安全、SQLite/普通文件；遇到设计冲突、来源不可验证或安全边界无法满足时停止并报告。
```

## 10. 可直接发送给 UI 项目对话的任务指令

```text
按照仓库中的 Official UI Plugin Alpha 设计基线和《Capability Bus 两项目总体开发设计与交接方案》开发 capability-bus-official-ui。

先读取 AGENTS.md、README、全部设计/规划文档，检查 Git HEAD、工作区和测试状态。架构保持不变：官方 UI 是可安装、可授权、可启停、可卸载的系统插件，不是核心内置前端；只能使用公开 Capability，禁止访问核心数据库或私有 API。

先执行 U0，只完成信息架构、状态模型、权限矩阵、交互流程、错误/空状态、验收测试设计和工程决策。核心未提供 F1-CONTRACT-FREEZE.md、机器可读契约、mock/fake core 和 conformance suite 前，不得猜测接口并开始业务实现。

F1 完成后依次执行 U1、U2、U3；核心 F2 完成后执行 U4 真实联调。所有写操作必须受权限约束并生成审计关联；禁用或卸载 UI 后核心必须继续独立运行。

UI 如需核心新增接口，必须提出公开、通用、可供任意插件使用的契约变更，不得要求 UI 专属后门。保留用户已有修改；遇到协议缺失或兼容性硬阻塞时停止并报告。
```

## 11. 顶层协调项目建议

可以建立第三个独立项目作为整体调度与发布协调层，但不建议直接把整个 `Codes` 父目录作为一个项目，因为其中可能包含无关仓库，权限、索引范围和误操作半径都会过大。

建议建立同级仓库：

```text
Codes/
├── personal-ai-control-plane/       # 来源项目，维护期
├── capability-bus-core/             # 核心代码
├── capability-bus-official-ui/      # UI 插件代码
└── capability-bus-platform/         # 只做协调、契约索引和发布治理
```

`capability-bus-platform` 只维护：

- 总路线、里程碑和跨项目 Definition of Done；
- F1/F2 冻结记录；
- 核心/UI 版本兼容矩阵；
- 跨项目 ADR、风险、集成测试计划和发布清单；
- 两个项目的可验证状态链接或固定 commit；
- 联合发布证据。

它不拥有核心权威状态，不复制实现代码，也不成为运行时依赖。整体调度对话可以在此项目中检查两个项目状态、编排开发顺序并生成交接指令；实际代码修改仍应在各自项目对话中完成。

Codex 中不同项目对话彼此独立。用户已将批准路线内的工作流授权和项目对话调度权委托给协调层，具体规则见 `docs/governance/platform-delegated-authority.md`。协调对话可以按门禁自动创建、继续和核验 Core/UI 对话；实际代码修改仍由对应项目对话在所属仓库完成。遇到该规则列出的强制人工检查点时必须停止。

## 12. 总体验收标准

整体 Alpha 路线完成必须同时满足：

- 核心可在没有 UI、Agent、审批、知识库和网络的情况下独立运行；
- UI 以普通协议包安装、授权、启停和卸载；
- 既有通用代码完成可追溯抽取，不是重新实现；
- 两项目以机器可读契约和 conformance suite 协作；
- F1 前 UI 不猜测接口，F2 后不扩张范围；
- Provider、UI 和功能插件均可替换且稳定系统 ID 不变；
- 默认安全、可恢复、可审计，且符合本机资源约束；
- 所有声明均有 Git commit、测试结果或运行证据支持。
