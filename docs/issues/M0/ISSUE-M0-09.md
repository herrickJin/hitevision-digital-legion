# ISSUE-M0-09: Agent Instance 状态机规格

## Parent
PRD-102 §4.4

## What to build
定义 Agent Instance 7 状态状态机：待审批、待派发、运行中、已停止、已删除、已回收、已拒绝。完整转换规则表（12 条）、Mermaid 状态图、数据保留策略。

## Acceptance criteria
- 覆盖 PRD-102 §4.4 全部 7 状态和 12 条转换规则
- 待审批为唯一初始态
- 已删除/已回收/已拒绝为绝对终态
- 运行中⇄已停止每次记录审计日志

## Blocked by
None — 可立即开始
