# ISSUE-M0-06: 对象关系 ER 图

## Parent
PRD-100 §6 核心概念模型

## What to build
基于 ISSUE-M0-01 ~ ISSUE-M0-05 的对象定义，绘制完整 Mermaid ER 图：
1. 核心对象之间的一对多、多对多、一对一关系
2. 外键关系和级联策略
3. 关系基数标注
4. 按 PRD-100 §6.2 分层展示（定义层→项目层→运行层→协作层→知识层）

关键关系：
- Soul Prototype 1:N Agent Instance
- Project 1:N Agent Instance
- Project 1:N TaskIssue
- Agent Instance 1:N TaskIssue
- TaskIssue 1:N TaskEvent / TaskArtifact
- TaskIssue 0:1 TaskIssue（追问链）
- Project 1:N Project Member / Project Memory Item

## Acceptance criteria
- 使用 Mermaid ER 图语法
- 覆盖 M0-01 ~ M0-05 定义的所有对象
- 每条关系有明确基数标注
- 按层级分组展示
- 包含关系说明文档

## Blocked by
- ISSUE-M0-01, M0-02, M0-03, M0-04, M0-05
