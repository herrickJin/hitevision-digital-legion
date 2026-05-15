# ISSUE-M1-16: RAG 参数配置

## Parent
PRD-101 §4.3, PRD-105 §4.1

## What to build
实现 RAG 参数配置：1.文档分段策略选择（按段落/按章节/自定义） 2.检索参数配置（top_k/相似度阈值/检索模式） 3.检索测试（输入查询词查看检索结果）。

## Acceptance criteria
- 分段策略至少 3 种
- 检索参数有合理默认值
- 检索测试可实时查看效果
- top_k 默认 5 相似度阈值默认 0.7

## Blocked by
- ISSUE-M1-05
