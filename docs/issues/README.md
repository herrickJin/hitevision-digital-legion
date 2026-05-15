# 数字军团OS Issue 索引

## 说明

本目录存放数字军团OS项目的垂直切片 Issue 文档。每个 Issue 是一条端到端的可交付切片，可被独立认领和验收。

> **[一级需求总览与排期建议](PRD-Requirements-Overview.md)** — 包含 R1~R10 全部一级需求、56 项二级需求矩阵、依赖关系和排期建议。

## 里程碑

| 里程碑 | 切片数 | HITL | AFK | 状态 |
|--------|--------|------|-----|------|
| M0 方案对齐 | 22 | 4 | 18 | 待开始 |
| M1 定义中心 | 27 | 1 | 26 | 待开始 |

## M0 方案对齐

### 对象模型组

- [ISSUE-M0-01](M0/ISSUE-M0-01.md) — Project 对象 P0 字段定义
- [ISSUE-M0-02](M0/ISSUE-M0-02.md) — Soul Prototype 对象 P0 字段定义
- [ISSUE-M0-03](M0/ISSUE-M0-03.md) — Agent Instance 对象 P0 字段定义
- [ISSUE-M0-04](M0/ISSUE-M0-04.md) — TaskIssue 对象 P0 字段定义
- [ISSUE-M0-05](M0/ISSUE-M0-05.md) — 辅助对象定义（Member/Snapshot/Event/Artifact 等）
- [ISSUE-M0-06](M0/ISSUE-M0-06.md) — 对象关系 ER 图

### 状态机组

- [ISSUE-M0-07](M0/ISSUE-M0-07.md) — Project 状态机规格
- [ISSUE-M0-08](M0/ISSUE-M0-08.md) — Soul 原型状态机规格
- [ISSUE-M0-09](M0/ISSUE-M0-09.md) — Agent Instance 状态机规格
- [ISSUE-M0-10](M0/ISSUE-M0-10.md) — TaskIssue 状态机规格
- [ISSUE-M0-11](M0/ISSUE-M0-11.md) — 状态机通用模式提取

### 联动约束组

- [ISSUE-M0-12](M0/ISSUE-M0-12.md) — Project 状态对 Instance/Issue 的联动约束
- [ISSUE-M0-13](M0/ISSUE-M0-13.md) — Soul 原型状态对 Instance 的联动约束
- [ISSUE-M0-14](M0/ISSUE-M0-14.md) — Instance 状态对 Issue 的联动约束

### 规范组

- [ISSUE-M0-15](M0/ISSUE-M0-15.md) — API 路由命名规范
- [ISSUE-M0-16](M0/ISSUE-M0-16.md) — 请求响应 Envelope 与分页规范
- [ISSUE-M0-17](M0/ISSUE-M0-17.md) — 错误码分段体系
- [ISSUE-M0-18](M0/ISSUE-M0-18.md) — UI 状态枚举规范（加载/空/失败/禁用）

### 技术决策组

- [ISSUE-M0-19](M0/ISSUE-M0-19.md) — 前端技术栈 ADR
- [ISSUE-M0-20](M0/ISSUE-M0-20.md) — 后端技术栈 ADR
- [ISSUE-M0-21](M0/ISSUE-M0-21.md) — 数据存储选型 ADR（关系型 + 向量）
- [ISSUE-M0-22](M0/ISSUE-M0-22.md) — 多终端 Runtime 架构 ADR

## M1 定义中心

### 数据层

- [ISSUE-M1-01](M1/ISSUE-M1-01.md) — Soul Prototype 存储层 + CRUD API
- [ISSUE-M1-02](M1/ISSUE-M1-02.md) — Soul Prototype 配置快照存储

### 列表与导航

- [ISSUE-M1-03](M1/ISSUE-M1-03.md) — 原型列表页（搜索/筛选/分页/排序）
- [ISSUE-M1-04](M1/ISSUE-M1-04.md) — 原型详情页（配置总览/状态/版本历史）

### 创建与编辑 — 分步表单

- [ISSUE-M1-05](M1/ISSUE-M1-05.md) — 新建向导框架（分步导航 + 草稿保存）
- [ISSUE-M1-06](M1/ISSUE-M1-06.md) — 基础信息配置
- [ISSUE-M1-07](M1/ISSUE-M1-07.md) — 人格定义配置
- [ISSUE-M1-08](M1/ISSUE-M1-08.md) — 职责范围配置
- [ISSUE-M1-09](M1/ISSUE-M1-09.md) — 输出规范配置
- [ISSUE-M1-10](M1/ISSUE-M1-10.md) — 安全约束配置

### 能力绑定

- [ISSUE-M1-11](M1/ISSUE-M1-11.md) — Body 选择器与绑定
- [ISSUE-M1-12](M1/ISSUE-M1-12.md) — Skills/MCP/Tools 能力绑定与预检
- [ISSUE-M1-13](M1/ISSUE-M1-13.md) — 载体兼容性映射与降级规则配置

### 上下文与知识

- [ISSUE-M1-14](M1/ISSUE-M1-14.md) — Wiki 文档引用绑定
- [ISSUE-M1-15](M1/ISSUE-M1-15.md) — 初始记忆配置
- [ISSUE-M1-16](M1/ISSUE-M1-16.md) — RAG 参数配置

### 校验与测试

- [ISSUE-M1-17](M1/ISSUE-M1-17.md) — 必填字段校验 + 配置结构校验
- [ISSUE-M1-18](M1/ISSUE-M1-18.md) — 依赖兼容性校验
- [ISSUE-M1-19](M1/ISSUE-M1-19.md) — 测试沙箱执行引擎
- [ISSUE-M1-20](M1/ISSUE-M1-20.md) — 测试结果结构化展示

### 状态机与版本

- [ISSUE-M1-21](M1/ISSUE-M1-21.md) — Soul 原型 8 状态机实现
- [ISSUE-M1-22](M1/ISSUE-M1-22.md) — 版本号管理（Major/Minor/Patch）
- [ISSUE-M1-23](M1/ISSUE-M1-23.md) — 配置快照与版本 diff 对比

### 审核与发布

- [ISSUE-M1-24](M1/ISSUE-M1-24.md) — 提交审核（附测试报告 + 配置快照）
- [ISSUE-M1-25](M1/ISSUE-M1-25.md) — 审核人界面
- [ISSUE-M1-26](M1/ISSUE-M1-26.md) — 上架/下架/归档操作
- [ISSUE-M1-27](M1/ISSUE-M1-27.md) — 审计日志记录
