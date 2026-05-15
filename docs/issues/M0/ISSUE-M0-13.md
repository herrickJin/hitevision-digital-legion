# ISSUE-M0-13: Soul 原型状态对 Instance 的联动约束

## Parent
PRD-101 §4.5, PRD-102 §4.6

## What to build
定义 Soul 原型状态变更对已派发 Instance 的联动约束：已上架→已下架触发实例回收、版本更新通知策略（Patch/Minor/Major 不同策略）、已归档强制回收。含更新前快照和失败回滚。

## Acceptance criteria
- 覆盖原型状态变更对实例的全部影响场景
- Patch/Minor/Major 更新有不同通知和确认策略
- 包含更新失败回滚流程
- 包含 Mermaid 时序图

## Blocked by
ISSUE-M0-06, ISSUE-M0-08
