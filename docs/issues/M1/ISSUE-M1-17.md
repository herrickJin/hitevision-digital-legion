# ISSUE-M1-17: 必填字段校验 + 配置结构校验

## Parent
PRD-101 §4.5.3, §9 验收标准 1

## What to build
实现完整性校验引擎：1.必填字段校验（name/role_type/personality/scope/skill_set/tool_set） 2.配置结构校验（人格定义/职责范围/输出规范结构完整性） 3.校验结果结构化展示（通过/未通过/警告） 4.未通过校验的原型不得提交测试。

## Acceptance criteria
- 必填字段校验覆盖全部 P0 必填项
- 校验结果按字段分组展示
- 未通过项有具体修改建议
- 校验不通过时提交按钮禁用

## Blocked by
- ISSUE-M1-06, M1-07, M1-08, M1-09, M1-10
