# Core T1 抽取与去产品化复核报告

状态：`platform_verified`；后续阶段：`T2_authorized`

核验日期：2026-10-01（Asia/Shanghai）

本报告是按开发进度保留的 T1 复核证据。T1 不是强制人工接受门；Platform 根据已委托的工作流权限完成独立核验并放行 T2。F1 仍是下一项强制人工检查点，Official UI 继续保持 U0。

## 授权与提交边界

| 项目 | 已核验状态 |
|---|---|
| Platform T1 授权基线 | `128e67b02c234ac47467daa1f865a238ccfca4a6` |
| Core T1 起点 | `aa6269842fa034c8849c579b0262dff4db7ee8cc` |
| Core 原样抽取提交 | `675667a` |
| Core T1 最终提交 | `cf7e87d814ad97d15b100c42c53a606b273b8fd5` |
| 固定来源 | `personal-ai-control-plane@7f82511837adf06eef87dd9059b57d59bcaeadd6` |
| Official UI | `7365453781aecdf7909d8d46282c0d09e8050c3a`，工作区干净，仍为 U0 |

Core 从 T0 最终提交到 T1 最终提交只有两笔提交，明确区分原样抽取与后续适配。最终 Core 工作区干净。来源 HEAD 未变，原有 4 个已修改项和 10 个未跟踪状态项原样保留；Platform 未将其作为来源。UI 未发生提交或工作区变更。

## 原样抽取与来源证明

Platform 从固定来源提交的 Git 对象重新计算哈希，并与 Core `vendor/source-baseline/personal-ai-control-plane-7f82511/` 中的已跟踪文件逐一比较：18/18 文件 SHA-256 相同。该目录不在 `src/` 下，构建配置只包含 `capability_bus_core*` 与 `capability_bus_sdk*`。

Core 的原样抽取清单见 [t1-extraction-baseline.md](../../../../capability-bus-core/docs/extraction/t1-extraction-baseline.md)，逐项适配理由见 [t1-adaptation-report.md](../../../../capability-bus-core/docs/extraction/t1-adaptation-report.md)。原样目录中的 Task/Event/executor 等内容只作为 ADR-0001 后续系统插件证据，不是安装包或裸 Core 运行依赖。

## Platform 独立验证

Platform 使用来源项目现有 Python 3.9.6 环境离线重放，未下载依赖、未使用凭据、真实服务或用户数据：

| 验证项 | 结果 |
|---|---|
| 来源基线 event dispatcher | 11/11 通过 |
| 来源基线 task/executor | 9/9 通过 |
| 来源基线 authority evidence | 1/1 通过 |
| 适配后裸 Core 单元测试 | 39/39 通过 |
| `src` 与 `tests` 编译检查 | 通过 |
| 从最终 Git 提交归档离线构建/安装 | 通过 |
| 安装产物导入与版本 | Core/SDK 均为 `0.1.0-alpha.1` |
| 使用安装产物重跑 Core 测试 | 39/39 通过 |
| 安装源码产品名/动态加载边界扫描 | 无 `personal_ai`、RSS、Hermes、SiYuan、reference flow 或 `importlib` 引用 |

第一次直接从 Core 工作区重放安装时，Platform 的只读仓库边界阻止构建工具删除已有 `build/lib` 缓存；此前所有单元测试已通过。为排除权限与工作区缓存影响，Platform 随后从 `cf7e87d814ad97d15b100c42c53a606b273b8fd5` 的不可变 `git archive` 在临时目录重新构建、安装、导入并运行 39 项测试，全部通过。该初次失败归类为核验环境写权限限制，不是产品代码或构建定义失败。

## 架构与安全边界结论

- 授权默认拒绝，并发生在幂等查询/保留之前。
- 命令幂等逻辑身份由 caller、capability 与 idempotency key 组成，不依赖 Provider。
- Manifest 只描述 `process` 运行边界，T1 默认运行时使用 `NoExecutableProviders`，不加载未知插件。
- 注册项默认 `quarantine` 和 `disabled`；审计只记录 payload 字段名，不记录值。
- 安装包不含产品适配器、固定工作流、产品 CLI 或持久 Task/Event 执行实现。
- T1 只提供可安装骨架，不宣称已有进程沙箱、完整 Grant、持久 Registry、CLI 闭环或 F1 冻结。

威胁边界及非声明项见 [t1-threat-boundary.md](../../../../capability-bus-core/docs/security/t1-threat-boundary.md)。

## 阶段判定

- [x] 裸 Core 工程可离线构建和安装。
- [x] 原样抽取、来源哈希、目标路径、测试及适配理由可追溯。
- [x] 产品专属插件不是 Core 包或运行依赖。
- [x] 原样抽取与适配提交相互独立。
- [x] Core、UI、来源边界及用户修改均被保留。
- [x] T1 退出条件满足。
- [ ] T2 尚未实现；由 Platform 在本报告核验后单独调度。
- [ ] F1 尚未形成或接受；UI 业务实现仍禁止启动。

## 自动推进决定

未发现强制人工介入条件。Platform 依据委托授权放行 T2：实现 `0.1.0-alpha.1` 最小可执行闭环并准备 F1 证据包。T2 完成后 Platform 必须独立验证；F1 证据齐备时停止并提交人工复核，不得自动接受 F1 或开始 UI 业务实现。
