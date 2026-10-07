---
status: accepted
date: 2026-10-07
---

# OMP 作为原生 ACP 内置 Agent 接入

OMP（`@oh-my-pi/pi-coding-agent`）自带 ACP server：`omp acp` 在 stdio 上说话。它不是 Pi 的第二套适配器，也不走 `pi-acp`。

## Decision

- `AgentKind::Omp`（`"omp"`）是永久内置成员。`oh-my-pi` 读入时归一到同一身份。
- 拓扑是 `NativeAcp`。安装钉 `npm` 包 `@oh-my-pi/pi-coding-agent@18.8.0`，启动参数只有 `acp`。检测名是 `omp`。
- 发布物的 shebang 是 Bun。预检查要求 `bun >= 1.3.14`。Node/npm 只负责把包写进用户环境。
- 登录动作是 `omp login`。凭据在 OMP 自己的 vault 里。空的 `auth.json` 不能当成未登录，所以默认不拦会话启动。
- Skills 写到 `~/.omp/agent/skills`（`PI_CODING_AGENT_DIR` 可覆盖）。项目侧只声明已确认的 `.agents/skills`。
- 会话目录是工作区编码的文件夹，格式未确认。不把通用 jsonl 导入器指过去。

`registry_id = "omp"` 不是官方 Registry 条目。它只保证以后同名 Registry 条目绑到这个档案，而不是再造一个 Agent。
