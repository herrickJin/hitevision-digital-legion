# ADR-029：Body 安全沙箱——Agent 执行隔离策略

## 上下文

Agent（Phase 1 = Hermes）在执行 Issue 时会调用终端命令、读写文件、访问外部 API。这些操作有安全风险：任意代码执行、越权文件访问、数据外泄、恶意依赖引入。需要确定 Phase 1 的安全隔离程度。

Sidecar（ADR-025）插入后，安全管控多了一个执行层。需要明确 Agent 侧和 Sidecar 侧各自的安全职责。

## 决策

### Phase 1 策略：工作目录隔离 + 双层校验

**Docker 环境：** Agent 运行在容器内，天然获得文件系统/网络/进程命名空间隔离。

**PC 环境：** 通过工作目录限制 + 工具层路径校验实现隔离。

### 双层校验架构

| 层级 | 执行位置 | 作用 | 特征 |
|------|----------|------|------|
| Agent 侧软限制 | Agent 内部 | soul.md 约束 + 文件操作工具的路径规范 | 第一道防线，可理解相对路径和上下文，智能但可绕过 |
| Sidecar 侧硬限制 | Sidecar | 拦截 tool_call 事件中的越界操作 | 安全底线，即使 Agent 被绕过或替换仍然生效 |

### 工作目录隔离

Agent 的文件操作限定在项目工作目录内：

- Docker 环境：`/var/lib/body-agent/workspace/{project_id}/`
- PC 环境：`~/.body-agent/workspace/{project_id}/`

允许访问的路径：
- 项目工作目录及其子目录
- 临时目录（`/tmp` 或系统临时目录）
- Sidecar 指定的 artifacts 输出目录

禁止访问的路径：
- 工作目录外的用户文件
- 系统敏感目录（`/etc`、`/var/log`、`~/.ssh`、`~/.config`）
- 其他项目的工作目录

### Sidecar 侧拦截规则

Sidecar 监听 Agent stdout 的 `tool_call` 事件，对以下操作做路径校验：

1. **文件操作类 tool_call**：检查 `path` 参数是否在允许范围内。绝对路径前缀匹配。越界则丢弃 tool_call 并向 Agent 返回错误事件。
2. **终端命令**：不做路径级拦截，通过命令黑名单拦截危险命令。

Phase 1 命令黑名单：

```
sudo, su, chmod 777, chown,
rm -rf /, rm -rf ~, rm -rf *,
mkfs, dd if=/dev/zero,
shutdown, reboot, init,
curl | sh, wget | sh,
```

黑名单由 Sidecar 维护，平台可通过运行配置下发更新。

### Docker 环境加固

Docker 环境下 Sidecar 可选的容器安全配置：

```
--cap-drop ALL          # 移除所有 Linux capabilities
--read-only             # 只读根文件系统（工作目录用 volume）
--security-opt no-new-privileges
--pids-limit 256        # 限制进程数
--memory 2g             # 内存限制
```

Phase 1 不强制全部启用，按实际场景逐步收紧。

### Phase 1 不做

- 完整的 seccomp / AppArmor 安全策略
- 网络出站白名单（限制 Agent 可访问的外部域名）
- 依赖包安全扫描
- GPU 资源隔离
- 文件操作审计日志（Phase 2 在治理服务中补充）

## 被拒绝的替代方案

1. **无隔离，完全靠 Soul 约束**——Agent 直接运行在宿主机上，安全依赖 soul.md 的职责描述。被拒绝因为 Agent 可能被绕过或出错，没有技术层面的强制隔离是安全底线缺失。

2. **PC 环境也做容器沙箱**——即使 PC 环境也用 Docker 或 Firecracker 隔离 Agent。被拒绝因为要求用户 PC 安装 Docker，增加使用门槛；且文件系统映射增加复杂度。

3. **完整安全策略（seccomp/AppArmor）**——Phase 1 实施和调试成本高，且不同操作系统行为不一致。

## 后果

### 正面

- Docker 环境天然获得容器级隔离，零额外成本
- PC 环境通过工作目录 + 双层校验覆盖主要风险场景
- Sidecar 作为独立于 Agent 的安全层，Agent 替换不影响安全策略
- 命令黑名单可动态更新，不需要重启 Sidecar

### 负面

- PC 环境的隔离强度低于 Docker 环境——符号链接、hardlink 等可能绕过路径校验
- 命令黑名单是黑盒机制，无法覆盖所有危险命令变体
- Agent 侧软限制依赖 Agent 的配合，如果 Agent 不做路径校验则全靠 Sidecar 拦截
- Sidecar 拦截增加 tool_call 的延迟（每条都需要 Sidecar 检查）

## 复审条件

- 当发生安全事件（Agent 越权访问文件或执行危险命令）时
- 当 PC 环境隔离强度不足，需要引入轻量沙箱（如 nsjail、bubblewrap）时
- 当需要网络出站白名单时
- 当需要文件操作审计日志时
- 当 Agent 工具调用频率高导致 Sidecar 拦截成为性能瓶颈时
