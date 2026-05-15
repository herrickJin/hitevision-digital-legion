# ISSUE-M0-14: Instance 状态对 Issue 的联动约束

## Parent
PRD-102 §4.4, PRD-103 §4.4

## What to build
定义 Instance 状态变更对 Issue 的联动约束：运行中→已停止对活跃 Issue 的处理、已删除/已回收对关联 Issue 的终态化、心跳超时对活跃 Issue 的影响。含 Issue 执行中 Instance 被停止的保护机制。

## Acceptance criteria
- 覆盖 Instance 状态变更对 Issue 的全部影响场景
- 活跃 Issue 的 Instance 变更需特殊保护
- 包含 Issue 终态化后数据保留策略
- 包含 Mermaid 时序图

## Blocked by
ISSUE-M0-06, ISSUE-M0-09
