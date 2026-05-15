# ADR-025：Body 部署与分发机制——Sidecar + Agent 混合架构

## 上下文

ADR-001 定义 Body = Hermes Agent。但将 Body 绑定到具体 Agent 实现有三个问题：

1. 如果未来需要替换 Agent（如 Claude Code、Codex），平台和 Body 的耦合需要全部重做。
2. Agent 的管控能力（心跳、缓存、副作用拦截、升级）混在 Agent 内部，职责不清晰。
3. 不同 Agent 的适配成本不可控——每个 Agent 都要重新实现平台通信协议。

需要确定 Body 的部署形态、分发机制，以及 Agent 可插拔的架构边界。

## 决策

**Body = Sidecar（管控层）+ Agent（执行引擎），Sidecar 对 Agent 可插拔。Phase 1 Agent = Hermes Agent。**

### 架构模型

```
平台运行服务 ←──WebSocket──→ Sidecar ←──stdin/stdout NDJSON──→ Agent 子进程
                                 │
                                 ├── 生命周期管理（启动/停止/重启）
                                 ├── 平台通信（心跳/事件上报/指令接收）
                                 ├── 安全管控（副作用拦截/事件缓存/补传）
                                 └── 升级管理（自身+Agent）
```

### 部署形态

Phase 1 支持两种用户侧环境：

| 制品 | 目标环境 | 分发方式 |
|------|----------|----------|
| Docker 镜像 | 服务器 Docker 环境 | 平台镜像仓库，用户 `docker pull` |
| 桌面安装包 | PC（macOS / Windows / Linux） | 平台下载页，用户手动安装 |

Phase 1 不做：K8s Helm Chart、免安装 Web Agent、移动端。

### Sidecar ↔ Agent 本地通信协议

**Agent stdout → Sidecar**（换行分隔 JSON 事件流）：

| type | 用途 |
|------|------|
| `heartbeat` | 存活探测 |
| `tool_call` | 工具调用记录（含 side_effect 标记，供审计和拦截） |
| `tool_result` | 工具调用结果 |
| `artifact` | 产物产出通知（文件路径引用，二进制不走 stdout） |
| `status` | 任务状态变更 |
| `log` | 日志输出 |

**Sidecar stdin → Agent**（换行分隔 JSON 指令）：

| type | 用途 |
|------|------|
| `task` | 下发任务 + 上下文 |
| `cancel` | 取消任务 |
| `config_update` | 运行时配置更新 |

每个事件必须包含 `type` 和 `ts`（时间戳）字段。二进制/大文件 Agent 写到约定目录（`artifacts/`），stdout 只传路径引用。

### 职责划分

| 职责 | Sidecar | Agent |
|------|---------|-------|
| 平台 WebSocket 连接 | ✓ | |
| 心跳维持 | ✓ | |
| 任务下发（翻译为 stdin 指令） | ✓ | |
| 执行事件收集与上报 | ✓ | |
| 终端输出流转发 | ✓ | |
| Agent 生命周期管理 | ✓ | |
| 副作用拦截（ADR-023） | ✓ | |
| 事件缓存与补传（ADR-016） | ✓ | |
| 版本上报（Sidecar + Agent + Soul） | ✓ | |
| 升级执行（自身 + Agent） | ✓ | |
| 实际推理和工具调用 | | ✓ |
| Soul 加载和 Skill 执行 | | ✓ |
| 本地文件操作 | | ✓ |

原则：Sidecar 不碰业务逻辑，只管连接、管控、安全、生命周期。Agent 只管执行。

### 认证机制

Phase 1 采用注册 Token + WebSocket 认证：

1. 实例派发时平台生成 `instance_token`，和 `instance_id` 一起通过 API 返回给用户
2. Sidecar 启动时携带 `instance_id` + `instance_token` 作为 WebSocket 连接的认证参数
3. 平台校验 token 有效性后建立连接
4. Token 一次一实例，实例回收后 token 失效

Docker 环境：环境变量注入 `INSTANCE_ID` + `INSTANCE_TOKEN`。
PC 环境：`body-agent init` 交互式配置或命令行参数传入。

### Sidecar 启动配置

最小启动配置（4 项）：

| 参数 | 来源 | 说明 |
|------|------|------|
| `platform_url` | 启动注入或交互式配置 | 平台 WebSocket 地址 |
| `instance_id` | 启动注入或交互式配置 | 实例标识 |
| `instance_token` | 启动注入或交互式配置 | 认证凭据 |
| `agent_command` | 启动注入或交互式配置 | Agent 启动命令（如 `hermes run`） |

连接平台后拉取的运行配置（不落盘）：`heartbeat_interval`、`cache_max_events`、`cache_max_size_mb`、`log_level`、`side_effect_tools`。

配置入口支持两种：
- **交互式命令**：`body-agent init` 引导配置，写入本地 `config.yaml`
- **环境变量/命令行参数**：Docker 环境通过 `-e` 注入，脚本通过 `--platform-url` 传入

### 本地文件布局

```
~/.body-agent/                      # Sidecar 根目录（可配置）
├── config.yaml                     # 启动配置（平台地址、instance_id、token）
├── agent/                          # Agent 二进制/入口
│   └── (hermes 可执行文件或软链接)
├── cache/                          # 断连事件缓存（补传后清理）
│   └── events/
├── artifacts/                      # Agent 产物输出目录
├── logs/                           # Sidecar + Agent 日志
└── backup/                         # 升级前备份（升级成功后可清理）
```

Docker 环境中根目录为 `/var/lib/body-agent/`，关键目录映射到 volume。

### 升级策略

**Sidecar 升级：**
- 平台通过 WebSocket 推送 `upgrade_available`（携带下载地址 + 哈希校验）
- Sidecar 下载 → 校验 → 备份旧版到 `backup/` → 替换 → 重启
- Docker 环境：拉取新镜像 → 停旧容器 → 启新容器
- 首次连接时平台校验版本兼容性，不兼容则 Sidecar 回滚

**Agent 升级：**
- Sidecar 管控 Agent 版本。平台知道每个 Soul 版本需要的最低 Agent 版本
- 升级流程：平台推送 → Sidecar 下载 → 重启 Agent 子进程（不需要重启 Sidecar）

### Agent 接入标准（Adapter 模式）

新 Agent 接入时实现一个 Adapter，必须提供：

1. 启动 Agent 子进程的方式
2. 将平台 task 指令翻译为 Agent 输入格式
3. 将 Agent 输出翻译为标准 NDJSON 事件流
4. Agent 进程健康检查

Phase 1 内置 Hermes Adapter，通过 `agent_type` 参数指定。Phase 2 可支持动态加载 Adapter 插件。

## 被拒绝的替代方案

1. **Body = 纯 Hermes Agent**——不加 Sidecar，管控能力直接做在 Hermes 里。被拒绝因为绑定具体 Agent 实现，未来替换成本高；且管控逻辑和执行逻辑耦合。

2. **Sidecar 和 Agent 通过本地 HTTP API 通信**——Agent 暴露 REST 端口，Sidecar 调用。被拒绝因为要求每个 Agent 实现一套 API 服务端，适配成本高；且 Docker 环境需要暴露额外端口。

3. **Body 完全平台托管**——Agent 运行在平台服务器集群。被拒绝因为 Phase 1 平台需要承担所有用户的计算资源成本和运维复杂度。

4. **Body 完全用户自管**——用户自行下载安装 Agent，平台只提供连接信息。被拒绝因为版本碎片化、运维不可控、无法保证 Agent 和 Soul schema 的兼容性。

## 后果

### 正面

- Agent 可插拔，平台不绑定具体 Agent 实现
- Sidecar 职责清晰，只管管控，Agent 只管执行
- 进程级集成对 Agent 改造要求最低（stdin/stdout NDJSON），新 Agent 接入成本低
- Hermes Agent Phase 1 已有适配基础，不增加额外开发量
- Sidecar 可以在不重启 Agent 的情况下升级自身

### 负面

- 用户侧多了一个进程需要管理（Sidecar），部署从"一个 Agent"变成"Sidecar + Agent"
- Agent Adapter 层引入了输入输出的翻译开销，可能丢失 Agent 特有的丰富输出格式
- Docker 环境中 Sidecar 和 Agent 在同一容器内，Sidecar 崩溃会导致 Agent 一起终止
- 升级机制（下载+替换+重启）在 PC 环境中可能遇到权限问题和文件锁定问题

## 需要更新的已有 ADR

| ADR | 更新内容 |
|-----|----------|
| ADR-001 | Body 定义从"Hermes Agent"改为"Sidecar + Agent，Phase 1 Agent = Hermes" |
| ADR-009 | 上下文组装后通过 Sidecar stdin 协议转发给 Agent，不直接注入 Agent |
| ADR-015 | Body 的 WebSocket 端点从 Agent 直连改为 Sidecar 代理，增加 Sidecar ↔ Agent 本地协议层 |
| ADR-016 | 事件缓存从 Agent 侧移到 Sidecar 侧，副作用拦截在 Sidecar 侧执行 |
| ADR-023 | side_effect 拦截由 Sidecar 执行（根据 Agent 上报的 tool_call 事件中的标记） |

## 复审条件

- 当需要接入第二个 Agent（验证 Adapter 模式的可扩展性）时
- 当 Sidecar 和 Agent 同容器部署的稳定性成为问题时（考虑拆分为 Sidecar 容器 + Agent 容器的 Pod 模式）
- 当 PC 环境升级的权限问题频繁出现时
- 当需要支持 K8s 部署形态时
- 当 Agent stdout 的 NDJSON 协议无法满足新 Agent 的输出需求时
