# ADR-011：Issue 完成后的双层沉淀策略

## 上下文

PRD-103 要求"最终结果默认沉淀回 Project Output 或 Project Memory"。需要明确沉淀的内容和存储位置。

## 决策

**Issue 完成后进行双层沉淀：原始结果存 TaskArtifact，经验摘要存 Project Memory。**

### 沉淀流程

```
Issue 完成
  → 原始结果 → TaskArtifact
    → 存储在 Issue 产物区
    → 可追溯、可引用、可下载
    → 挂载到相关工作台模块
  
  → 经验摘要 → Project Memory Item
    → 由 Body 在任务完成时生成结构化总结
    → 标记为自动写入，来源指向原 Issue
    → 可被后续 Issue 的 RAG 检索命中
```

### 举例

前端研发完成"修改登录页交互"Issue：
- **TaskArtifact**：完整的代码变更记录、截图、走查结果
- **Project Memory Item**："本项目登录页使用 OAuth 2.0 + 手机验证码双因素认证，前端路由为 /login，组件位于 src/pages/Login/"

### Phase 1 简化

- 经验摘要由 Body 自动生成，不需要人工审核
- 标记来源为 auto，指向 Issue ID
- P1 再引入人工审核和过期清理机制

## 被拒绝的替代方案

1. **只存原始结果**——被拒绝因为完整 PRD 文档或代码变更不适合作为 Memory 检索内容，噪音太大。

2. **只存经验摘要**——被拒绝因为原始结果需要保留用于追溯和引用。

3. **人工审核后才写入 Memory**——被拒绝因为 Phase 1 增加使用摩擦，先用自动写入 + 来源标记。

## 后果

### 正面

- 原始结果保留完整，支持追溯
- 经验摘要精炼，适合 RAG 检索
- 两者存储位置不同，互不干扰

### 负面

- Body 需要在任务完成时额外生成经验摘要，增加执行时间和 Token 消耗
- 自动生成的摘要质量依赖 Agent 能力

## 复审条件

- 当 Project Memory 检索命中率低时（摘要质量不够）
- 当 P1 引入审核机制时
