---
status: accepted
date: 2026-10-07
---

# OMP 作为原生 ACP 内置 Agent 接入

OMP（`@oh-my-pi/pi-coding-agent`）自带 ACP server：`omp acp` 在 stdio 上说话。它不是 Pi 的第二套适配器，也不走 `pi-acp`。

## Decision

- `AgentKind::Omp`（`"omp"`）是永久内置成员。`oh-my-pi` 读入时归一到同一身份。
- 拓扑是 `NativeAcp`。安装钉 `@oh-my-pi/pi-coding-agent@18.8.0`，启动参数只有 `acp`。检测名是 `omp`。
- 发布物的 shebang 是 Bun。预检查要求 `bun >= 1.3.14`。Node/npm 只负责把包写进用户环境。
- 登录动作是 `omp login`。设置面只有官方订阅这一档，密钥不走环境变量。活凭据在 `~/.omp/agent/agent.db` 的 `auth_credentials`，自定义端点的 `apiKey` 在 `models.yml`。空的 `auth.json` 不是未登录。有 oauth 记为账号，只有 api key 记为密钥。两者都没有时预检查失败。
- 原生配置只编辑 `config.yml` 的 `modelRoles.default`。不把 `models.yml` 整文件放进设置快照，避免把密钥回传给界面。
- ACP 会话的 MCP 只来自 `session/new.mcpServers`。OMP 在 ACP 里关掉磁盘 `.mcp.json` 发现，所以不把 OMP 做成原生 MCP 文件目标。
- Skills 写到 `~/.omp/agent/skills`（`PI_CODING_AGENT_DIR` 可覆盖）。项目侧是 `.agents/skills`。
- 历史在 `~/.omp/agent/sessions/<编码工作区>/*.jsonl`。记录形状与 Pi 的 `session` / `message` 相同，标题来自第一条 `title` 记录。

`registry_id = "omp"` 不是官方 Registry 条目。它只保证以后同名 Registry 条目绑到这个档案，而不是再造一个 Agent。
