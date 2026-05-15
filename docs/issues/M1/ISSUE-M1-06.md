# ISSUE-M1-06: 基础信息配置

## Parent
PRD-101 §4.1

## What to build
实现基础信息配置步骤：名称（≤50字）、角色类型（枚举选择）、业务域、适用场景、负责人。含实时校验和字段提示。

## Acceptance criteria
- 名称必填且≤50字
- 角色类型枚举与 ISSUE-M0-02 对齐
- 所有必填字段有红色标记
- 校验不通过时禁用下一步

## Blocked by
- ISSUE-M1-05
