# ISSUE-M0-12: Project 状态对 Instance/Issue 的联动约束

## Parent
PRD-100 §5.3, PRD-102 §4.4, PRD-103 §4.4

## What to build
定义 Project 状态变更对 Instance 和 Issue 的联动约束矩阵：筹备中→进行中校验 P0 必备项、进行中→已暂停冻结运行、→已归档实例回收+Issue终态化。含联动原子性和失败回滚策略。

## Acceptance criteria
- 覆盖 Project 4 状态对 Instance 和 Issue 的全部联动场景
- 每个联动有触发条件/影响范围/失败策略
- 包含 Mermaid 时序图
- 明确同步/异步执行策略

## Blocked by
ISSUE-M0-06, ISSUE-M0-07
