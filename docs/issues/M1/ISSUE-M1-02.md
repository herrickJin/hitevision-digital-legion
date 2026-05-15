# ISSUE-M1-02: Soul Prototype 配置快照存储

## Parent
PRD-101 §4.7 版本管理

## What to build
实现 Soul Prototype 的配置快照存储能力：
1. Prototype Version Snapshot 存储表（基于 ISSUE-M0-05）
2. 快照创建 API（保存当前完整配置）
3. 快照列表查询 API（按 prototype_id 查询版本列表）
4. 快照详情查询 API（按 prototype_id + version 查询）
5. 快照与原型的关联关系

## Acceptance criteria
- 快照包含原型的全部配置字段
- 快照不可修改，只可创建和查询
- 快照列表按版本号倒序排列
- 快照存储使用 JSON 格式保存完整配置

## Blocked by
- ISSUE-M0-02（字段定义）
- ISSUE-M0-05（辅助对象定义）
- ISSUE-M1-01（存储层基础）
