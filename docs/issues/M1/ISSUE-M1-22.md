# ISSUE-M1-22: 版本号管理（Major/Minor/Patch）

## Parent
PRD-101 §4.7

## What to build
实现版本号管理：1.版本号自动生成规则（Major/Minor/Patch） 2.版本变更类型选择 3.变更说明编辑 4.版本号与配置快照关联。

## Acceptance criteria
- Major/Minor/Patch 有明确的变更规则
- 每次版本变更自动创建配置快照
- 已上架版本不可修改
- 版本号遵循 semver 规范

## Blocked by
- ISSUE-M1-02, ISSUE-M1-21
