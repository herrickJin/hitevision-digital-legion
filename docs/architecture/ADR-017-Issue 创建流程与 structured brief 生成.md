# ADR-017：Issue 创建流程与 structured brief 生成

## 上下文

PRD-103 定义用户输入自然语言任务描述，系统自动提取 structured brief，用户确认后创建 Issue。需要确定谁来做自然语言到结构化的转换，以及完整的创建流程。

## 决策

### structured brief 由平台侧统一生成

- 不依赖具体 Body（Body 的上下文组装在 Issue 执行时才做）
- 平台侧有通用 LLM 调用能力，负责任务结构化
- 统一 brief 的 schema 和校验逻辑

### 完整创建流程

```
1. 用户在工作台输入自然语言任务描述
   ↓
2. 前端将文本发送到平台任务服务 API
   ↓
3. 平台调用 LLM 生成 structured brief
   - 提取：任务标题、任务目标、预期产出、优先级、涉及对象
   - 输出：结构化 JSON
   ↓
4. 平台做越界预检
   - 查询当前数字员工实例对应的 Soul 的 scope 定义
   - 判断任务是否在职责范围内
   - 如果越界，直接返回拒绝建议
   ↓
5. 返回 structured brief 给前端展示
   - 用户可查看、修改、补充
   ↓
6. 用户确认
   ↓
7. 创建 Issue（状态：待开始）
   ↓
8. 用户确认发起执行
   ↓
9. 平台触发上下文组装（ADR-009）+ 规划检查（ADR-010）
   ↓
10. 进入执行中
```

### structured brief schema（Phase 1 最小集）

```json
{
  "title": "任务标题（≤100字）",
  "objective": "任务目标描述",
  "expected_output": "预期产出类型（文档/代码/测试/分析/方案）",
  "priority": "低/中/高/紧急",
  "scope_hint": "涉及的对象范围（仓库/文档/接口/组件）",
  "attachments": ["附件引用列表"],
  "supplementary": "用户的补充说明"
}
```

## 被拒绝的替代方案

1. **Body 侧生成 structured brief**——让当前数字员工实例自己理解任务。被拒绝因为 Body 在 Issue 创建时还未完成上下文组装，不具备完整的角色认知。且不同 Body 的结构化能力不一致，平台侧统一更可控。

2. **前端直接解析**——前端 NLP 模型做结构化。被拒绝因为前端不应承担业务逻辑，且 NLP 能力有限。

## 后果

### 正面

- structured brief 的质量统一，不受 Body 差异影响
- 越界预检在创建阶段完成，减少无效 Issue
- 平台侧可以统一优化 prompt 和结构化质量

### 负面

- 平台侧需要维护 LLM 调用能力，增加平台复杂度
- structured brief 的质量依赖 LLM 能力，可能需要迭代优化 prompt

## 复审条件

- 当 structured brief 质量不达标时
- 当需要支持多语言任务描述时
