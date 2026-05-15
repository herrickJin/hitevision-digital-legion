# ISSUE-M0-16: 请求响应 Envelope 与分页规范

## Parent
PRD-100 §11.4

## What to build
定义统一的 API 请求响应规范：请求/响应 Envelope 格式、分页规范（page/limit + cursor）、过滤与搜索参数、批量操作格式。参照 PRD-100 §11.4.1 确保 API 响应可驱动前端四种状态。

## Acceptance criteria
- 定义统一 JSON 响应 envelope
- 分页支持 page/limit 和游标两种模式
- 错误响应包含 error_code + message + detail

## Blocked by
None — 可立即开始
