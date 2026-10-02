# F1 Contract Freeze 人工复核报告

状态：`waiting_for_human_acceptance`

复核日期：2026-10-02（Asia/Shanghai）

候选 Core 提交：`78127bb9192aa6ddf8f15b544c553c149415c5a5`

本报告是 F1 的强制人工检查点。Platform 已完成技术核验，但无权自动接受 F1。接受必须绑定上述精确提交；接受前不得启动 Core T3 或 Official UI U1/业务实现。

## 阶段与提交证据

| 里程碑 | Core 提交 | 说明 |
|---|---|---|
| T1 最终基线 | `cf7e87d814ad97d15b100c42c53a606b273b8fd5` | 可安装的去产品化骨架 |
| T2 可执行闭环 | `198dd7c9c3f4e4ad44aad1ef1d1a294afa162865` | 持久状态、进程插件、CLI 与离线场景 |
| 初始 F1 候选 | `604f8df13671fc740a210df7659b0122a874fbda` | Schema、示例、fake Core 与候选文档 |
| 进程清理竞态修复 | `1f3a240c530fb86f89e2d344dc138812092d01f1` | 异常握手与管道清理顺序 |
| Platform 复核修正 | `42652bad0600bc9c61e5c2e19c7055888ec3cd56` | Grant/Provider 绑定及 Schema/runtime 一致性 |
| 最终证据 | `78127bb9192aa6ddf8f15b544c553c149415c5a5` | 不可变提交复跑记录，工作区干净 |

Core 对话：`01a0f7c7-8a3b-78d2-adaa-8f1f83c3f34e`。执行模型为 GPT-5.6 Sol，推理强度 high。Core、Platform、Official UI 最终工作区均干净。

## Platform 独立复核结果

Platform 未仅接受 Core 对话结论，而是从最终 Git 提交的不可变归档离线构建和复跑：

| 核验项 | 结果 |
|---|---|
| 最终源码完整套件 | 71/71 通过 |
| 隔离安装产物完整套件 | 71/71 通过 |
| 安装产物专项契约/授权/故障/CLI 套件 | 27/27 通过 |
| Python 编译检查 | 通过 |
| wheel 离线构建与隔离安装 | 通过 |
| 安装边界 | 仅 Core、SDK、入口与元数据；无 vendor、tests、examples、产品模块 |
| 原样来源文件 | 18/18 与 `personal-ai-control-plane@7f825118` SHA-256 相同 |
| 来源仓库 | HEAD 未变；原有 14 项未提交状态保留 |
| Official UI | `7365453781aecdf7909d8d46282c0d09e8050c3a`，工作区干净，仍为 U0 |

Core 自身还记录了 20 个独立进程、共 200 次 malformed-handshake 清理压力循环通过。Platform 的最终专项套件包含相同故障路径的回归测试。

## Platform 发现并验证的修正

Platform 在初始候选 `1f3a240` 上实际复现：插件 A 的旧 Grant 在 A 被移除、插件 B 以不同 system ID 接管同名 capability 后，能够错误调用 B。初始候选因此未被接受。

最终候选将当前路由 Provider 的稳定 system ID 纳入授权决策。Platform 再次手工重现场景，结果为：

```text
旧 Provider Grant → GRANT_PROVIDER_DENIED
拒绝期间启动的新 Provider 进程数 → 0
新 Provider 自有 Grant → 调用成功
```

同一逻辑插件的版本替换继续保留 system ID、Grant 和 Provider-neutral 幂等结果。拒绝发生在幂等预留和插件进程启动之前。

初始候选的 Schema 只被解析、未真正应用于实例。最终候选新增零网络、零第三方依赖的受限 Draft 2020-12 验证器，实际执行候选使用的本地 `$ref`、严格类型、required、additionalProperties、properties、enum/const、pattern、长度/最小值、items/uniqueItems、anyOf 和带时区 date-time；未知关键字、远程引用和目录逃逸均失败关闭。Schema 与 runtime 共同拒绝未知字段及字符串到 bool/int 的静默转换。

## F1 候选内容

- 11 个版本化 JSON Schema：Manifest、Request/Result/Event/Error Envelope、ContentRef、SecretRef、Capability Descriptor、Grant、生命周期和审计分页。
- 10 个合法示例、4 个非法边界示例和 1 个 `GRANT_PROVIDER_DENIED` 结构化错误示例。
- 真实 alpha.1 CLI：裸核心、未知进程插件、Grant、生命周期、能力调用、审计、重启和移除闭环。
- dependency-free conformance suite 和确定性的 JSONL fake Core。
- 插件进程握手、超时、崩溃、协议污染、消息上限和清理测试。
- 持久注册表、稳定 system ID、Grant 过期/撤销/Provider 绑定、授权先于幂等、Provider-neutral 原子幂等和元数据审计。
- lifecycle 词汇包含 `discovered`、`registered`、`disabled`、`enabled`、`starting`、`healthy`、`degraded`、`failed`、`stopped`、`removed`；alpha.1 的 `discovered/registered` 是非持久化校验/原子注册瞬时状态，未知插件落盘后为 `quarantine + disabled`。
- 兼容与弃用候选规则：F1 后破坏性变更必须升级协议版本、提供迁移说明、更新示例/一致性测试并记录 Core/UI 兼容性。

Core 权威候选索引见 [F1-CONTRACT-FREEZE-CANDIDATE.md](../../../../capability-bus-core/docs/releases/F1-CONTRACT-FREEZE-CANDIDATE.md)，契约说明见 [contracts/v0.1/README.md](../../../../capability-bus-core/contracts/v0.1/README.md)，验证原始摘要见 [t2-verification.md](../../../../capability-bus-core/docs/testing/t2-verification.md)。

## 明确限制与未冻结内容

- alpha.1 使用一次调用一进程会话；`start` 是握手/就绪探测并记录逻辑健康状态，不代表常驻进程恢复。
- 进程边界不是 OS 沙箱；插件仍可能访问宿主用户可访问的文件或网络。OS 级隔离属于后续阶段。
- fake Core 冻结候选只提供 UI U1/U2 所需的只读管理能力；真实管理写操作目前为 CLI-local。UI U3 的写入管理协议须在后续 Core 阶段完成并于 F2 前冻结。
- 尚未实现完整 Trust/Identity/resource-scope policy、Secret Store 注入、配置历史、常驻恢复、持久 Task/Event/workflow、产品适配器、MCP 或 UI。
- F1 接受不等于 F2、发布候选或生产安全批准。

## 请求人工决定

请确认是否接受以下完整决定：

```text
接受 capability-bus-core 提交
78127bb9192aa6ddf8f15b544c553c149415c5a5
作为 F1 Contract Freeze 基线；接受 alpha.1 一次调用一进程语义及其非 OS 沙箱限制；
接受当前只读管理 Capability/fake Core 足以启动 UI U1/U2，写入管理契约在后续 Core
阶段完成并于 F2 前冻结。授权 Platform 自动启动 Core T3 与 Official UI U1，直至下一强制人工检查点。
```

建议回复：`接受 F1`。如不接受，请指出需要修改的候选条款；Platform 将保持 Core T3 与 UI U1 停止。
