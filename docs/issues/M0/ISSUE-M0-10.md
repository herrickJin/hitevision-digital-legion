# ISSUE-M0-10: TaskIssue 状态机规格

## Parent
PRD-103 §4.4

## What to build
定义 TaskIssue Phase 1 七状态状态机：待开始、执行中、等待补充信息、已完成、执行失败、已拒绝、已取消。完整转换规则表（8 条）、重试机制、追问链预留、Mermaid 状态图。

## Acceptance criteria
- 覆盖 PRD-103 §4.4 全部 7 状态和 8 条转换规则
- 执行失败根据 retry_count 区分条件性终态
- 等待补充信息→执行中恢复在原 Issue 完成
- Phase 1 不引入自动超时

## Blocked by
None — 可立即开始
