# ADR-006：能力模型分层——Skill、Tool、MCP、Function Calling

## 上下文

Soul 压缩包中包含 skills/、tools.yaml、MCP 配置、Function Calling 定义。Hermes Agent 本身也自带基础能力。需要明确这几个概念的层次关系和职责边界。

## 决策

### 四层能力模型

| 层次 | 概念 | 定义 | 存储位置 | 示例 |
|------|------|------|----------|------|
| 知识层 | Skill | 指导 Agent "怎么做"的 prompt 级知识 | Soul 压缩包 skills/ | code-review.md、prd-writing.md |
| 能力层 | Tool | Agent 可调用的具体动作入口 | 能力注册中心 + Soul tools.yaml 引用 | git、terminal、Figma、Swagger |
| 协议层 | MCP | 外部系统接入的标准协议 | Soul mcp/ 配置 + MCP Server | 知识库接入、IM 接入 |
| 调用层 | Function Calling | 针对特定 API 的结构化调用定义 | Soul functions/ | REST API 调用、GraphQL 调用 |

### 关系定义

```
能力注册中心
  ├── 注册所有 Tool 定义
  │   ├── 平台预置通用工具（git、terminal、http）
  │   ├── 角色特定工具（Figma、Swagger、Open Design、压测工具）
  │   ├── MCP Server 暴露的工具
  │   └── Function Calling 定义的工具
  └── 每个 Tool 关联一个标准工作台模块类型

Soul 压缩包
  ├── tools.yaml 从能力注册中心选择需要的 Tool
  ├── skills/ 放置 Skill 文件（不经过能力注册中心）
  ├── mcp/ 放置 MCP Server 连接配置
  └── functions/ 放置 Function Calling 定义
```

### 层次关系

1. **Skill 独立于 Tool**——Skill 是"怎么做"的知识，Tool 是"用什么"的能力。Skill 引导 Agent 如何使用 Tool。
2. **MCP 是 Tool 的来源之一**——MCP Server 暴露的工具注册到能力注册中心，成为可被 tools.yaml 引用的 Tool。
3. **Function Calling 是 Tool 的一种形态**——结构化 API 调用定义也是一种 Tool，注册到能力注册中心。
4. **能力注册中心统一管理所有 Tool 的元数据**——包括权限要求、模块映射、适用载体、副作用标记（`side_effect`）等。

### 副作用标记

每个 Tool 在能力注册中心注册时声明 `side_effect` 属性：

- `side_effect: true`——该工具会产生外部副作用（远端写操作、对外通知、改变外部状态等）
- `side_effect: false`（默认）——该工具仅产生本地效果

`side_effect` 标记的用途：
- Sidecar 在失联时根据此标记拦截副作用工具调用（ADR-023）
- 审计日志中标记副作用操作
- 前端工作台展示副作用操作的特殊状态

参见 ADR-023（失联副作用控制）和 ADR-029（安全沙箱）。

## 被拒绝的替代方案

1. **Skill 和 Tool 统一注册**——Skill 也进能力注册中心。被拒绝因为 Skill 是 prompt 级知识，没有可调用的动作入口，注册中心的元数据模型不适用。

2. **MCP 和 Function Calling 不区分**——统一为"外部能力接入"。被拒绝因为两者技术协议不同，配置方式不同，运行时加载机制不同。

## 后果

### 正面

- 层次清晰，每层有明确职责
- 能力注册中心作为 Tool 的唯一元数据源，避免散落
- Skill 和 Tool 解耦，可以独立演进

### 负面

- 能力注册中心成为关键依赖——如果注册中心不可用，Soul 压缩包的工具引用无法解析
- 新增 Tool 类型需要更新能力注册中心的元数据 schema

## 复审条件

- 当需要支持新的工具类型（如设备控制工具）时
- 当 Skill 需要动态加载（不打包在 Soul 里）时
