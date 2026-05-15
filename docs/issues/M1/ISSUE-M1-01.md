# ISSUE-M1-01: Soul Prototype 存储层 + CRUD API

## Parent
PRD-101 §4.1 原型基础定义

## What to build
实现 Soul Prototype 对象的存储层和基础 CRUD API，打通数据持久化全链路：
1. 数据库表创建（基于 ISSUE-M0-02 字段定义）
2. CRUD API 实现（POST/GET/PUT/DELETE）
3. 字段校验（必填/类型/长度/枚举值）
4. 基础错误处理（遵循 ISSUE-M0-17 错误码）
5. API 路由遵循 ISSUE-M0-15 命名规范
6. 响应格式遵循 ISSUE-M0-16 Envelope 规范

## Acceptance criteria
- CRUD 四个操作全部可用
- 必填字段校验生效（name/role_type/owner_id/status/version）
- 枚举字段校验生效（role_type/status/risk_level）
- 错误响应使用标准错误码
- API 响应使用标准 Envelope

## Blocked by
- ISSUE-M0-02（字段定义）
- ISSUE-M0-15（API 命名）
- ISSUE-M0-16（响应规范）
- ISSUE-M0-17（错误码）
