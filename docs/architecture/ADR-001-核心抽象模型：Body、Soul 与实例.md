# ADR-001：核心抽象模型——Body、Soul 与实例

## 上下文

数字军团OS 需要一个清晰的核心抽象来连接"数字员工定义"、"项目引入"和"任务执行"三个层面。PRD 定义了 Body、Soul、数字员工实例三个概念，但技术层面的绑定机制和运行模型需要明确。

PRD-100 将 Body 定义为"可运行的通用智能体空壳"，将 Soul 定义为"数字员工的专业身份定义"，但未明确两者的技术本质和组合方式。

## 决策

采用 **Body + Soul + 运行时上下文** 的三层组合模型：

```
数字员工实例 = Body + Soul 版本引用 + Project 运行时上下文
```

具体定义：

1. **Body = Sidecar（管控层）+ Agent（执行引擎）**（ADR-025）
   - **Sidecar** 是独立守护进程，负责平台通信、心跳、事件缓存补传、副作用拦截、升级管理和 Agent 生命周期管理
   - **Agent**（Phase 1 = Hermes Agent）负责实际推理、工具调用和中间产物生成
   - Sidecar 对 Agent 可插拔，不绑定具体 Agent 实现
   - Sidecar 与 Agent 通过 stdin/stdout NDJSON 协议交互

2. **Soul = 自包含的结构化工具压缩包**
   - 包含：soul.md（人格/职责/约束）、tools.yaml（工具集 + 模块映射）、skills/（技能文件）、memory/（初始记忆）、MCP 配置、Function Calling 定义
   - 是可版本化、可审核、可测试的配置资产
   - 不包含运行时状态

3. **数字员工实例 = Sidecar + Agent + Soul 版本引用 + Project 运行时上下文注入**
   - Sidecar 管理与平台的连接和 Agent 的生命周期
   - Agent 启动时通过 Sidecar 加载 Soul 压缩包（Sidecar 下载缓存，转给 Agent 加载）
   - 实例绑定一个 Soul 版本引用（非 Soul 内容本身）
   - Project 上下文在 Issue 执行时通过 Sidecar 动态组装并注入 Agent（ADR-009）
   - 实例归属于唯一 Project

4. **绑定机制**
   - Soul 是配置资产，Sidecar + Agent 是运行时
   - 绑定 = Sidecar 下载 Soul 压缩包缓存到本地 → Sidecar 通过 stdin 协议将上下文注入 Agent → Agent 加载 Soul 生成 System Prompt + Tool 注册 + Memory 注入
   - 三者通过 Soul schema 和 Sidecar↔Agent NDJSON 协议解耦

## 被拒绝的替代方案

1. **Body 和 Soul 紧耦合**——Soul 定义直接嵌入 Body 代码。被拒绝因为无法支持多角色、无法版本化管理、无法独立测试。

2. **Soul 只是数据库记录**——人格/职责/约束存在数据库各表，运行时从多个表拼装。被拒绝因为 Soul 无法作为独立资产传输、测试和版本化。

3. **打包时注入全部上下文**——实例派发时把 Soul + Project Objective + Wiki + Memory 一次性打包。被拒绝因为 Wiki 和 Memory 会持续更新，打包后知识会过时。

## 后果

### 正面

- Body 和 Soul 解耦，Sidecar 和 Agent 各自独立演进，Agent 可插拔替换
- Soul 作为自包含资产，天然支持版本化、审核、测试、分发
- 运行时动态加载保证实例始终使用最新的 Project 上下文
- 与 Hermes Agent 的现有架构一致，不需要改造运行时核心
- Sidecar 作为管控层解耦了平台通信和业务执行

### 负面

- 需要一个"上下文组装层"在 Issue 执行时动态合成完整运行配置（上下文组装在 Sidecar 中执行）
- Soul 压缩包的 schema 定义需要向前兼容，版本升级时需考虑向后兼容性
- 运行时动态加载增加了 Issue 执行前的延迟（上下文组装耗时）
- Sidecar 和 Agent 之间的 NDJSON 协议需要版本管理，协议不兼容时需要 Sidecar + Agent 同时升级

### 风险

- Soul schema 版本演进可能与 Agent 的加载能力不同步。缓解：schema 变更走版本号管理，Sidecar 按 schema 版本号协调 Agent 加载。

## 复审条件

- 当 Hermes Agent 发生重大版本变更时
- 当需要支持新的载体类型（移动端、设备侧）时
- 当 Soul schema 需要不兼容变更时
- 当需要接入第二个 Agent 实现（验证可插拔性）时（参见 ADR-025）
