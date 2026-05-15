# ADR-027：API 认证与租户上下文传递

## 上下文

平台有 6 个服务（ADR-014），前端和 Sidecar 两种调用方，需要统一的认证机制。ADR-018 决定数据模型预留多组织（tenant_id），API 层需要从认证 token 中提取租户上下文。ADR-025 定义 Sidecar 使用 instance_token 连接平台。需要确定完整的认证方案和上下文传递方式。

## 决策

### 双通道认证

平台有两种调用方，使用不同认证方式：

| 调用方 | 认证方式 | Token 内容 |
|--------|----------|------------|
| 前端（用户浏览器） | JWT（JSON Web Token） | user_id, tenant_id, org_role |
| Sidecar（Body 管控层） | instance_token | instance_id |

### 前端认证流程

```
用户登录（用户名/密码或 SSO）
  → 平台认证 API（独立端点或由治理服务承担）
  → 签发 JWT
    payload: {
      user_id: "u_xxx",
      tenant_id: "t_xxx",
      org_role: "admin" | "member",
      exp: 1715826000
    }
  → 前端存储 JWT（localStorage 或 cookie）
  → 后续每次请求 Authorization: Bearer <JWT>
```

### Sidecar 认证流程

```
实例派发时平台生成 instance_token
  → 通过 API 返回给用户（Docker 环境变量 / PC 命令行参数）
  → Sidecar 启动时携带 instance_id + instance_token
  → WebSocket 连接时作为认证参数传递
  → 平台运行服务校验 token 有效性
  → 实例回收后 token 失效
```

### API Gateway 职责

**Gateway 只做认证（Authentication），不做授权（Authorization）。**

| 职责 | 说明 |
|------|------|
| JWT 校验与解析 | 验证签名、过期时间，提取 payload |
| 租户上下文注入 | 将 user_id、tenant_id、org_role 注入请求头，透传给下游服务 |
| Sidecar instance_token 校验 | 验证 token 有效性，提取 instance_id |
| 路由转发 | 按路径规则转发到目标服务 |
| 全局限流 | 全局级别的请求频率控制 |

### 授权下沉到各服务

各服务根据自身业务语义做授权检查：

- **治理服务**：校验 org_role 是否有权限操作组织级资源
- **项目服务**：校验用户在项目中的角色（Owner/Member/Viewer）
- **任务服务**：校验用户是否可以创建/取消 Issue（基于项目角色）
- **运行服务**：校验 instance_token 是否匹配有效实例

### JWT 设计约束

- Phase 1 access token 有效期 24 小时
- Phase 1 不做 refresh token 和 token 撤销
- JWT 签名密钥由平台配置管理，支持轮换
- JWT payload 不包含项目级角色（项目角色需要实时查询，不适合放进 token）

### Phase 1 不做

- mTLS 双向证书认证
- OAuth2 设备授权流
- Token 自动轮换
- Refresh token
- 单点登录（SSO）集成（预留接口，Phase 2 接入企业 IdP）

## 被拒绝的替代方案

1. **Session + Cookie**——平台维护 session 存储。被拒绝因为 6 个服务需要共享 session，增加存储依赖和一致性问题；JWT 无状态更轻量。

2. **Gateway 做认证 + 授权**——Gateway 同时做粗粒度权限拦截。被拒绝因为项目级权限需要查成员关系，Gateway 不应该有这个认知；且权限模型演进会导致 Gateway 频繁变更。

3. **统一 Token 类型**——前端和 Sidecar 用同一种 token。被拒绝因为两种调用方的身份模型完全不同（用户身份 vs 实例身份），强行统一会增加复杂度。

## 后果

### 正面

- JWT 无状态，6 个服务不需要共享 session 存储
- 双通道认证适配两种调用方特征，不强行统一
- Gateway 职责简单，稳定不频繁变更
- 租户上下文通过 JWT payload 天然携带，ADR-018 的 tenant_id 预留有实现基础

### 负面

- JWT 有效期内无法撤销（用户被踢出组织后 token 仍然有效直到过期）。缓解：Phase 1 有效期 24h，影响可控；Phase 2 引入 token 黑名单
- 项目级角色不在 JWT 中，每次项目操作需要额外查询。缓解：查询频率不高，可缓存

## 复审条件

- 当需要接入企业 SSO（LDAP/OIDC）时
- 当需要 token 撤销能力时
- 当 JWT 有效期安全问题凸显时（考虑 refresh token + 短有效期 access token）
- 当需要 Sidecar token 轮换时
