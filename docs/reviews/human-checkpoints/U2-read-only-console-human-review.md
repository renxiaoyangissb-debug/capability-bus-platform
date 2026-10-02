# Official UI U2 只读控制台复核报告

状态：`platform_verified`；后续阶段：`U3_not_started_by_current_dispatch_scope`

核验日期：2026-10-02（Asia/Shanghai）

本报告保留 Official UI U2 的独立复核与一次拒收修正证据。U2 不是强制人工接受门；Platform 根据已委托权限完成验收。本次重新调度明确只完成 U2，不开始 U3、真实 Core 联调或 F2。

## 提交与工作区

| 项目 | 已核验状态 |
|---|---|
| UI U1 起点 | `562828ebb7a760066844c172c23b3b2200abae5a` |
| 初始 U2 候选 | `74131c529b4e6e4d0fffa695a65feb8b7208cf9d` |
| 离线确定性修正 | `f17320cf39f18d507a4e40b0f7047b88a14582ae` |
| UI 最终工作区 | 干净，`main` |
| Core F1 契约来源 | `78127bb9192aa6ddf8f15b544c553c149415c5a5` |
| Platform 拒收检查点 | `53a6470a02db0719de2063e0c1f1c71aa895586d` |

U2 实现 Dashboard、Plugins、Capabilities、Runtime、Audit 五个只读页面及 ready、loading、empty、error、offline、incompatible、forbidden 七类状态。数据只经 F1 冻结的七个 read capability 进入 read model。

## 初始候选拒收与修正

Platform 在全新临时副本复核 `74131c5` 时发现，原 bootstrap 会根据缓存中碰巧存在的当前平台 optional tarball 重新求解依赖：缓存增加 `lightningcss-darwin-arm64` 后，锁文件 SHA-256 从 `302048…` 漂移到 `efa22f…`，依赖数量从 19 变 20，插件包清单从 30 变 31；标准 `npm ci --offline` 同时因锁文件缺少 optional 节点失败。功能测试通过，但离线可复现证据不成立，因此候选被拒收。

修正提交将版本与完整性选择完全固定到提交内 `package-lock.json`，补齐跨平台 optional 节点；额外缓存版本被忽略，当前平台所需 tarball 缺失或完整性不符时明确失败。三项新增回归测试分别覆盖缓存额外版本、缺包和完整性错误。

## Platform 独立验证

Platform 从 `f17320c` 创建全新临时 clone，未复用仓库 `node_modules`、`dist` 或 `build`：

| 验证项 | 结果 |
|---|---|
| bootstrap 前锁文件 SHA-256 | `c8ef2875aa4c2ae0e7c0a62dd4c9c0081f6527b6ea6194c108f6f5a73bef38e5` |
| `npm run bootstrap:offline` | 固定安装 20 包；锁文件字节不变 |
| `npm ci --offline --ignore-scripts --no-audit --no-fund` | 成功安装 20 包 |
| 冻结契约核验 | 27 个文件与 6 个来源身份匹配 Core F1 |
| 类型检查与测试 | 通过；22/22 测试 |
| Vite 生产构建 | 通过；22 个模块 |
| 依赖许可证 | 20/20 有声明 |
| 源码与插件边界检查 | 12 项通过 |
| 插件包完整性 | 31/31 文件通过 |
| Core F1 契约目录相对 U1 | 零差异 |

## 架构、交互与安全边界

- 页面不含安装、启停、调用、Grant、配置、retry、dead-letter 或其他 mutation 控件。
- Runtime 只投影公开 status 与 plugin lifecycle 字段，不虚构 PID、日志或资源遥测接口。
- 只调用 `system.status.get`、plugins list/inspect、capabilities list/describe、grants list 与 audit list。
- 不导入 Core 包，不访问 Core SQLite 或私有 API，不连接真实 Core。
- 临时视觉预览仅绑定 `127.0.0.1` 且已停止；交付插件进程保持 HTTP-inert。
- 保留键盘焦点、跳转链接、语义表格、ARIA 状态和 reduced-motion 支持。
- Secret 值不进入 UI 边界，只允许 `secretref://` 引用显示。

## 阶段判定

- [x] 五个 U2 只读页面与七类状态完成。
- [x] 只使用 F1 冻结公开 read capability 与 fake Core。
- [x] mock 驱动测试、错误码与 trace 保真通过。
- [x] 离线依赖、锁文件、许可证与插件包结果可确定性重建。
- [x] 无私有状态访问、真实 Core 接入、公网监听或写操作。
- [x] UI 工作区干净，提交可验证。
- [ ] U3 未开始；本次调度不授权自动进入 U3。
- [ ] F2/U4 未开始，F2 仍是强制人工接受门。

## 当前决定

Platform 接受 `f17320cf39f18d507a4e40b0f7047b88a14582ae` 作为已验证的 Official UI U2 交付。UI 对话停止在 U2 完成点；在新的明确调度前不得开始 U3、真实 Core 联调或 F2。
