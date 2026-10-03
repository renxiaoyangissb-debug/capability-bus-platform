# Capability Bus 插件接入接口说明

适用版本：Core `0.1.0-alpha.3`，含已验证的 T5 管理引导兼容补充 `3225038`

调用协议版本：`0.1.0-alpha.1`

管理协议版本：`0.1.0-alpha.3`
保存日期：2026-10-03（Asia/Shanghai）

## 1. 插件模型

插件是独立目录、独立进程和独立状态所有者，不是 Core Python 模块。

当前约束：

- 运行入口必须是包内相对路径的 Python `.py` 文件。
- Core 使用自己的 Python 解释器启动插件。
- 插件不得 import Core 实现、打开 Core SQLite 或访问其他插件数据库。
- 插件通过 stdin/stdout 上的一行一个 JSON（JSONL）通信。
- 业务代码、产品流程、工作流和领域数据全部留在插件。
- `plugin.yaml` 当前实际由 JSON 解析器读取，因此请写成 JSON 格式，不要使用普通 YAML 语法。

建议目录：

```text
my-plugin/
├── plugin.yaml
├── plugin.py
├── schemas/
│   ├── config.json
│   ├── input.json
│   └── output.json
└── my_existing_code/
    └── ...
```

## 2. Manifest

最小示例：

```json
{
  "manifest_version": "0.1.0-alpha.1",
  "id": "acme.document-tools",
  "version": "0.1.0-alpha.1",
  "kind": "tool",
  "runtime": {
    "type": "process",
    "command": "plugin.py",
    "args": []
  },
  "provides": [
    {
      "capability": "document.convert",
      "operation": "command",
      "input_schema": "schemas/input.json",
      "output_schema": "schemas/output.json"
    }
  ],
  "consumes": [],
  "permissions": {
    "network": [],
    "filesystem": {
      "read": [],
      "write": []
    },
    "workspaces": [],
    "secrets": []
  },
  "resources": {
    "max_concurrency": 1,
    "timeout_seconds": 60,
    "disk_min_free_mb": 256
  },
  "config_schema": "schemas/config.json"
}
```

关键字段：

| 字段 | 说明 |
|---|---|
| `id` | 稳定插件身份，小写字母、数字及 `._-` |
| `version` | 插件实现版本 |
| `kind` | 通常使用 `tool`、`system` 或 `interface` |
| `runtime.type` | 当前只能为 `process` |
| `runtime.command` | 包内 `.py` 入口，禁止绝对路径和 `..` |
| `provides` | 本插件提供的 Capability |
| `consumes` | 依赖声明；当前不等于已提供插件内调用客户端 |
| `operation` | `query` 或 `command` |
| `permissions` | 最小权限声明 |
| `resources` | 并发、超时和磁盘低水位 |
| `config_schema` | 包内配置 Schema 路径 |

Capability 名称必须至少包含一个点，例如：

```text
document.convert
mail.message.send
knowledge.entry.query
```

不要用名字猜测是否有副作用，必须显式声明：

- `query`：只读，不要求幂等键。
- `command`：可能产生副作用，调用时必须提供 `idempotency_key`。

## 3. 插件进程协议

Core 启动插件时只保证以下环境变量：

```text
CAPBUS_PLUGIN_ID
CAPBUS_PLUGIN_SYSTEM_ID
CAPBUS_PROTOCOL_VERSION
CAPBUS_PLUGIN_DATA_DIR
CAPBUS_PLUGIN_CACHE_DIR
PYTHONUNBUFFERED=1
PYTHONDONTWRITEBYTECODE=1
```

不要依赖用户的 `PATH`、`HOME`、代理或其他 shell 环境变量。

获得有效管理 Grant 的 resident `interface` 插件还会收到：

```text
CAPBUS_MANAGEMENT_SOCKET
CAPBUS_MANAGEMENT_PROTOCOL_VERSION
CAPBUS_MANAGEMENT_IDENTITY_ID
CAPBUS_MANAGEMENT_CREDENTIAL_ID
CAPBUS_MANAGEMENT_CREDENTIAL_GENERATION
CAPBUS_MANAGEMENT_HMAC_KEY
```

这些变量属于当前受监督进程的独立、可轮换管理会话，不是 owner 身份或 owner key。插件只能调用 Manifest `consumes` 中声明且被有效 Grant 授权的管理操作；声明本身不产生权限。停止、禁用、卸载、Trust Zone 变更、回滚、Grant 最终撤销、owner 凭据轮换、Core 重启或插件重新供给都会使旧凭据失效或安全失败。

管理请求使用 `CAPBUS_MANAGEMENT_HMAC_KEY` 对不含 `signature` 的规范 JSON（UTF-8、键排序、紧凑分隔符）执行 HMAC-SHA-256，然后通过 `CAPBUS_MANAGEMENT_SOCKET` 发送现有 `capbus-local-control-v1` 的 `management` envelope。插件不需要也不得 import Core、读取 Core SQLite、私有目录或 owner 凭据。

### 3.1 启动握手

插件启动后必须立即向 stdout 输出：

```json
{
  "type": "hello",
  "protocol_version": "0.1.0-alpha.1",
  "plugin_id": "acme.document-tools",
  "capabilities": ["document.convert"]
}
```

要求：

- 四个字段必须精确存在，不能增加额外字段。
- `plugin_id` 必须等于 Manifest ID。
- `capabilities` 必须与 `provides` 完全一致。
- 每条消息必须以换行结束并立即 flush。

### 3.2 接收调用

Core 发送：

```json
{
  "type": "request",
  "request": {
    "request_id": "req_...",
    "trace_id": "trace_...",
    "capability": "document.convert",
    "caller": {
      "subject_id": "owner:local"
    },
    "grant": {
      "grant_id": "grant_..."
    },
    "idempotency_key": "convert-2026-001",
    "payload": {
      "source": {
        "uri": "file:///workspace/input.md",
        "media_type": "text/markdown"
      }
    },
    "created_at": "2026-10-03T00:00:00Z",
    "protocol_version": "0.1.0-alpha.1"
  }
}
```

### 3.3 成功响应

插件输出：

```json
{
  "type": "result",
  "request_id": "req_...",
  "ok": true,
  "data": {
    "output": {
      "uri": "file:///workspace/output.pdf",
      "media_type": "application/pdf"
    }
  }
}
```

### 3.4 失败响应

```json
{
  "type": "result",
  "request_id": "req_...",
  "ok": false,
  "error": {
    "code": "DOCUMENT_CONVERSION_FAILED",
    "message": "Conversion failed",
    "retryable": false
  }
}
```

错误码只能使用大写字母、数字和下划线。当前 Core 会保留错误码和 `retryable`，但会把插件错误文本归一化为安全摘要，因此不要依赖插件错误消息向最终用户传递详细信息。

### 3.5 取消

Core 可能发送：

```json
{
  "type": "cancel",
  "request_id": "req_..."
}
```

插件可以协作式取消；即使不支持，也必须能承受 Core 随后终止整个插件进程组。请求已发送但结果不确定时，Core 会把副作用标记为 `unknown` 并要求显式 reconciliation，不会盲目重放。

### 3.6 传输限制

- 每条 JSONL 消息最大 1 MiB。
- stdout 只能输出协议消息。
- 日志写入 stderr。
- Core 不保存 stderr 正文，只记录有界字节计数和截断状态。
- 插件必须准备处理同一进程中的多个顺序请求。
- EOF 表示 Core 正在关闭连接。

## 4. 最小 Python 入口

```python
#!/usr/bin/env python3

import json
import os
import sys


def emit(value):
    sys.stdout.write(
        json.dumps(value, ensure_ascii=False, separators=(",", ":")) + "\n"
    )
    sys.stdout.flush()


def handle(capability, payload):
    if capability == "document.convert":
        # 在这里调用从现有项目抽取的实现。
        return {"accepted": True, "input": payload}
    raise ValueError("unsupported capability")


emit({
    "type": "hello",
    "protocol_version": os.environ["CAPBUS_PROTOCOL_VERSION"],
    "plugin_id": os.environ["CAPBUS_PLUGIN_ID"],
    "capabilities": ["document.convert"],
})

for line in sys.stdin:
    message = json.loads(line)

    if message.get("type") == "cancel":
        continue

    request = message.get("request", {})
    request_id = request.get("request_id", "unknown")

    try:
        if message.get("type") != "request":
            raise ValueError("invalid message")

        data = handle(
            request["capability"],
            request.get("payload", {}),
        )
        emit({
            "type": "result",
            "request_id": request_id,
            "ok": True,
            "data": data,
        })
    except Exception:
        emit({
            "type": "result",
            "request_id": request_id,
            "ok": False,
            "error": {
                "code": "PLUGIN_REQUEST_FAILED",
                "message": "Request failed",
                "retryable": False,
            },
        })
```

## 5. 数据、文件和秘密

### 插件状态

持久状态必须写到：

```python
data_dir = os.environ["CAPBUS_PLUGIN_DATA_DIR"]
```

临时缓存必须写到：

```python
cache_dir = os.environ["CAPBUS_PLUGIN_CACHE_DIR"]
```

不要在插件包目录中生成数据库、日志或缓存。Core 会保存整个包的内容哈希；注册后修改包内文件会触发 `PACKAGE_INTEGRITY_MISMATCH`。

### 大内容

大文件使用 `ContentRef`，不要塞进 JSON：

```json
{
  "uri": "file:///path/to/content",
  "media_type": "application/pdf",
  "sha256": "64位小写十六进制",
  "size": 12345
}
```

### 秘密

公开 Payload 只能包含引用：

```json
{
  "credential": {
    "uri": "secretref://my-provider/account"
  }
}
```

禁止传递 token、password、api_key 等明文。Core 的管理面只处理 `SecretRef` 注册、查询和撤销，不会把 SecretRef 等同于明文秘密注入；插件仍应保留自己的安全适配层。

## 6. 注册、授权与调用

推荐流程：

```bash
capbus --root /path/to/runtime init

capbus --root /path/to/runtime \
  plugin validate /absolute/path/to/my-plugin

capbus --root /path/to/runtime \
  plugin register /absolute/path/to/my-plugin

capbus --root /path/to/runtime \
  grant create \
  --subject owner:local \
  --plugin acme.document-tools \
  --capability document.convert

capbus --root /path/to/runtime \
  plugin enable acme.document-tools
```

需要 resident 运行时，可在单独终端启动：

```bash
capbus --root /path/to/runtime serve
```

然后启动插件并调用：

```bash
capbus --root /path/to/runtime \
  plugin start acme.document-tools

capbus --root /path/to/runtime \
  capability call document.convert \
  --subject owner:local \
  --grant grant_实际ID \
  --idempotency-key convert-2026-001 \
  --input '{"source":{"uri":"file:///tmp/input.md","media_type":"text/markdown"}}'
```

没有 `serve` 时，普通调用使用一次性受监督进程；有 `serve` 时，命令通过本地 Unix socket 转发并复用 resident 插件进程。

更新同一插件时：

1. 停止插件；
2. 更新包并提升版本；
3. 重新注册相同插件 ID；
4. 保持稳定的 `plugin_system_id` 和已有合法 Grant；
5. 重新启用、启动并验证。

## 7. 从现有项目迁移的推荐拆分

对每项既有能力先形成映射：

| 现有内容 | 迁移方式 |
|---|---|
| 纯业务函数 | 保留原实现，由协议适配层调用 |
| HTTP/数据库客户端 | 放入插件内部，声明相应权限 |
| 产品工作流 | 留在产品插件或独立编排插件 |
| Task/Event 持久化 | 插件自有状态，或通过独立可靠性系统插件提供 |
| Core 注册、授权、路由 | 不迁移，由 Core 负责 |
| UI | 独立 interface 插件，不进入 Core |
| 凭据 | 改成 `SecretRef` 边界 |
| 大文件 | 改成 `ContentRef` |
| 原项目数据库 | 只允许本插件访问自己的数据库，不与其他插件共享 |

迁移必须保留真实复用证据：

- 来源 Git commit；
- 原文件路径；
- 文件/blob 哈希；
- 对应测试；
- 目标路径；
- 适配理由。

不能只阅读旧代码后凭记忆重写。

## 8. Task/Event 能力边界

持久 Task、Event、checkpoint、lease、retry、backoff 和 dead-letter 不属于裸 Core。

当前参考实现是独立的 `system.durable-reliability` 插件，提供例如：

```text
reliability.task.submit
reliability.task.claim
reliability.task.checkpoint
reliability.task.pause
reliability.task.resume
reliability.task.complete
reliability.task.fail
reliability.event.publish
reliability.event.claim
reliability.event.ack
reliability.event.fail
reliability.dead_letter.list
```

你的插件不得 import 该插件或访问它的 SQLite。跨插件协作必须经过公开 Capability；当前 `consumes` 主要是声明，不应被当作已经存在的插件内调用 SDK。

## 9. 当前阶段必须注意的限制

- 仅支持 Python 进程入口；其他语言项目需要一个 Python 协议适配器。
- Manifest 中的网络、文件系统和 Secret 权限已有声明；Alpha 仍不把协议级权限声明等同于完整 OS 沙箱强制。
- Core 会检查 Schema 文件存在且位于包内，但插件仍应自行验证领域输入和输出，不能假设 Core 已执行每个插件 Schema。
- Core 已提供版本化配置、回滚与 SecretRef 管理；插件仍不得在协议载荷、日志或配置中传递明文秘密。
- 插件不得直接调用、import 或读取其他插件。
- UI 管理写接口已绑定到 F2 冻结的 42 操作管理目录；真实 Core/UI 联调属于 T5，尚未形成发布候选。
- F1 Schema 文件标题中的 `candidate` 是历史文本；F1 基线是 Core `78127bb9192aa6ddf8f15b544c553c149415c5a5`，F2 接受的完整 Alpha API 基线是 Core `272f6325506c4ce7c4de5c100fb3f03a28701318` 与 Official UI `39b5904499c0ecfa0dff1d57683812c150e377e4`。

## 10. 接入验收清单

- [ ] 插件包可被 `plugin validate` 接受。
- [ ] 包内无运行时生成文件。
- [ ] `hello` 与 Manifest 精确匹配。
- [ ] stdout 无日志或调试文本。
- [ ] 每项 Capability 明确为 `query` 或 `command`。
- [ ] command 有稳定的幂等策略。
- [ ] 输入和输出都有 JSON Schema，并在插件内验证。
- [ ] 所有状态只写入插件自己的 data/cache 目录。
- [ ] Payload 中没有明文秘密。
- [ ] 超时、崩溃、取消和重复调用测试通过。
- [ ] Core 重启后插件状态可恢复或明确失败。
- [ ] 插件禁用/移除后不影响裸 Core。
- [ ] 没有 Core/其他插件数据库或私有 API 访问。
- [ ] 来源提交、哈希、测试和适配理由完整记录。

## 权威参考

- Core 当前能力：`capability-bus-core/README.md`
- Manifest：`capability-bus-core/contracts/v0.1/plugin-manifest.schema.json`
- Request Envelope：`capability-bus-core/contracts/v0.1/request-envelope.schema.json`
- Result Envelope：`capability-bus-core/contracts/v0.1/result-envelope.schema.json`
- 最小进程插件：`capability-bus-core/examples/example-echo/`
- 可靠性系统插件：`capability-bus-core/system_plugins/durable-reliability/`

实际迁移前应先对来源项目做只读审计，生成“来源模块 → Capability → 插件包 → 测试”的迁移矩阵，再开始实施。
