# ISSUE-M0-04: TaskIssue 对象 P0 字段定义

## Parent
PRD-103 §4, PRD-100 §13.2.3

## What to build
定义 TaskIssue 对象的 P0 字段规范：标识、归属、描述、状态优先级、异常处理、时间创建人、追问链字段。

## Acceptance criteria
- 覆盖 PRD-100 §13.2.3 已有字段
- status 枚举包含 Phase 1 七状态
- retry_count 默认 0 上限 3
- structured_brief 使用 JSON

## Blocked by
None — 可立即开始
