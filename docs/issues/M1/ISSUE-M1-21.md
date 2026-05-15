# ISSUE-M1-21: Soul 原型 8 状态机实现

## Parent
PRD-101 §4.5

## What to build
实现 Soul 原型的 8 状态状态机：1.状态转换 API 2.转换校验（白名单机制） 3.转换触发方记录 4.可逆转换处理（已驳回→待测试） 5.转换审计日志写入。

## Acceptance criteria
- 状态转换严格遵循 ISSUE-M0-08 规格的 11 条规则
- 非法转换返回明确错误
- 每次转换记录触发方/时间/前后状态
- 状态字段不可直接修改（必须通过状态机 API）

## Blocked by
- ISSUE-M0-08, ISSUE-M1-01
