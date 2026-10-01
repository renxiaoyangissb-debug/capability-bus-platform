# Core T0 人工复核报告

状态：`waiting_for_human_review`

核验日期：2026-10-01（Asia/Shanghai）。本报告核验用户对 Platform `ddcfa0974637bbdd9a91d93b87f0a7f983eda8af` 批准的 T0 准备范围。T0 审计交付与技术证据核验已完成，人工接受尚未发生；T1/T2、F1/F2 接受和 UI 业务实现均未授权。

## 已核验的提交与状态

| 仓库 | 分支 | 提交 | 核验状态 |
|---|---|---|---|
| Platform 批准基线 | main | `ddcfa0974637bbdd9a91d93b87f0a7f983eda8af` | 审计派发前干净；本次新增协调授权与复核记录 |
| 来源 personal-ai-control-plane | main | `7f82511837adf06eef87dd9059b57d59bcaeadd6`，tag `v1.0` | 原有脏工作区保留，不纳入稳定来源 |
| Core T0 初始交付 | main | `9255224d7038ef569121d0bea3e2e2e1189b841f` | 新增五份 T0 审计/证据文件 |
| Core 最终交付 | main | `aa6269842fa034c8849c579b0262dff4db7ee8cc` | 仅追加治理测试计数表述修正，工作区干净 |
| Official UI | main | `7365453781aecdf7909d8d46282c0d09e8050c3a` | HEAD 与工作区保持不变，U0 设计准备 |

Core 对话：`01a0f7c7-8a3b-78d2-adaa-8f1f83c3f34e`，名称“Capability Bus Core T0 来源快照与复用审计”。已确认对话停止且空闲。Platform 未直接写入任何关联仓库；Core 交付由其所属项目对话完成。

## 交付文件

以下文件全部属于 Core 最终提交，Core 相对原始基线 `d84c495acbb69f588a5412f618952950fd4e5126` 的变更仅限这五个文件：

- [source-snapshot.md](../../../capability-bus-core/docs/extraction/source-snapshot.md)：来源身份、脏工作区隔离、环境和测试结果。
- [reuse-matrix.md](../../../capability-bus-core/docs/extraction/reuse-matrix.md)：逐项行为与测试对应、决策、理由和待授权的拟定目标路径。
- [provenance.md](../../../capability-bus-core/docs/extraction/provenance.md)：36 个来源文件的完整 commit、路径、Git blob 与 SHA-256。
- [source-head-unittest.txt](../../../capability-bus-core/docs/extraction/evidence/source-head-unittest.txt)：实际离线测试原始输出、命令、时间和退出码。
- [ADR-0001](../../../capability-bus-core/docs/adr/0001-t0-reuse-boundaries.md)：按 Core 权威设计区分控制平面与持久任务/事件系统插件；仅为 T0 分类，不改变架构或授权实现。

## 独立证据核验

Platform 重新读取最终 Git 提交范围与文件内容，而非仅接受对话的完成声明。36 行来源记录全部与来源完整提交的 Git blob 和内容 SHA-256 相符。复用矩阵覆盖主方案全部来源映射及 Core 设计的 11 项基础能力，明确区分 `extract`、`adapt`、`pluginize`、`retire` 和尚无合适来源的 `new` 工作。

来源工作区派发时有 14 条状态记录（4 个已修改文件，10 条未跟踪文件/目录记录）。Platform 在 T0 前后分别计算涉及的 21 个实际用户文件的 SHA-256，状态记录与文件哈希全部一致。来源 HEAD 保持不变。未提交设计/文档未被默认为稳定来源。

已查阅 Core 对话中实际测试执行的命令记录、退出码和输出，并与入库日志对照。测试从 `git archive HEAD` 的临时副本运行，使用来源现有验证环境；未安装依赖或调用真实产品服务。

```text
来源 HEAD：7f82511837adf06eef87dd9059b57d59bcaeadd6
临时副本：/tmp/capbus-source-head-rerun.uUeQug
Python：3.9.6
feedparser：6.0.12
命令：PYTHONDONTWRITEBYTECODE=1 PYTHONPATH=src /Users/butterburger/Documents/WorkPlace/Codes/personal-ai-control-plane/.venv/bin/python -m unittest discover -s tests -q
执行时间：2026-10-01 22:05:28–22:05:39（Asia/Shanghai）
结果：78 通过，0 失败，0 错误，0 跳过；退出码 0
```

这些测试验证来源旧产品行为。其中 40 项属于 RSS/Hermes/SiYuan 适配器测试，不能作为新 Core 契约、隔离、身份或安全能力已经实现的证明。没有创建 Core 运行时代码、T1 原样抽取基线或 T2 功能。

## 待人工复核的风险与决定

- [ ] 接受固定来源提交与排除未提交改动的取证边界。
- [ ] 确认来源代码的所有权及后续复用/分发许可依据。来源 HEAD 无仓库级 LICENSE/NOTICE，审计记录将此列为后续抽取前待解决事项；本报告不推定具体许可。
- [ ] 接受 ADR-0001 的复用归属分类：Core 保留控制平面、调用与审计元数据；持久 Task/Event、checkpoint、lease、retry、dead-letter 等按 Core 权威设计归属受闸门约束的系统插件。
- [ ] 接受安全适配要求：移除硬编码授权清单外放行与不受信插件同进程装载，补齐持久身份/Grant、Provider-neutral 幂等身份和原子调用预留。
- [ ] 接受证据缺口分类：完整 Envelope Schema、持久注册表、配置版本治理、Runtime Supervisor 和本地控制通道等不能被宣称为已复用完成，需要后续单独授权的新建或适配工作。
- [ ] 如决定继续，另行明确 T1 范围与退出条件。T0 的技术核验不自动授权 T1/T2，也不构成 F1 接受。

## 停止状态

`status=waiting_for_human_review`，`formal_development_authorized=false`。准备授权记录继续有效，但已授权 T0 执行已完成并停止。UI 维持 U0；F1/F2 均为 `not_started`。保留全部用户现有修改，等待人工复核决定。
