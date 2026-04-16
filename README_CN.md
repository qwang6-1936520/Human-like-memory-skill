# Human-Like Memory 集成仓库

[English](./README.md) | 中文

这个仓库现在提供 Human-Like Memory 的三种接入方式：

- OpenClaw
- Hermes Agent
- 直接 API 接入

你可以把它当成 Human-Like Memory 的统一集成入口，根据自己的运行环境选择对应方式。

## 三种接入方式

### 1. OpenClaw

如果你希望把 Human-Like Memory 接入 OpenClaw 工作流，走这一条。

- 接入文档：[docs/integrations/openclaw.md](./docs/integrations/openclaw.md)
- 官方文档：<https://plugin.human-like.me/docs?tab=plugin>
- OpenClaw 兼容元数据：[compat/openclaw/](./compat/openclaw/)

### 2. Hermes Agent

如果你希望通过 `recall`、`search`、`save`、`save-batch` 这些显式命令把长期记忆接入 Hermes Agent，走这一条。

- 接入文档：[docs/integrations/hermes-agent.md](./docs/integrations/hermes-agent.md)
- 官方文档：<https://plugin.human-like.me/docs?tab=hermes>
- Hermes Skill 清单：[SKILL.md](./SKILL.md)
- 运行时脚本：[scripts/memory.mjs](./scripts/memory.mjs)

### 3. 直接 API 接入

如果你希望从自己的应用、Agent Runtime 或后端服务里直接调用 Human-Like Memory，走这一条。

- 接入文档：[docs/integrations/api.md](./docs/integrations/api.md)
- 官方文档：<https://plugin.human-like.me/docs?tab=api>

## Human-Like Memory 能做什么

Human-Like Memory 为你的 Agent 栈提供显式的长期记忆操作：

- 检索相关的历史上下文
- 搜索过去的决策、偏好和事实
- 在信息值得保留时写入长期记忆
- 保持可控的记忆使用方式，而不是默认开启静默后台记忆

## 仓库结构

- `README.md`: 英文总入口
- `README_CN.md`: 中文总入口
- `docs/integrations/openclaw.md`: OpenClaw 接入说明
- `docs/integrations/hermes-agent.md`: Hermes Agent 接入说明
- `docs/integrations/api.md`: API 接入说明
- `SKILL.md`: Hermes Skill 清单
- `scripts/memory.mjs`: Hermes Skill 运行时
- `compat/openclaw/`: OpenClaw 兼容元数据

## 快速开始

### OpenClaw

快速开始：

```bash
export HUMAN_LIKE_MEM_API_KEY="mp_your_key_here"
export HUMAN_LIKE_MEM_BASE_URL="https://plugin.human-like.me"
export HUMAN_LIKE_MEM_USER_ID="openclaw-user"
export HUMAN_LIKE_MEM_AGENT_ID="main"
```

官方文档：
<https://plugin.human-like.me/docs?tab=plugin>

### Hermes Agent

快速开始：

```bash
hermes skills install github:qwang6-1936520/Human-like-memory-skill
export HUMAN_LIKE_MEM_API_KEY="mp_your_key_here"
export HUMAN_LIKE_MEM_BASE_URL="https://plugin.human-like.me"
export HUMAN_LIKE_MEM_USER_ID="hermes-user"
export HUMAN_LIKE_MEM_AGENT_ID="main"
node ./scripts/memory.mjs config
```

官方文档：
<https://plugin.human-like.me/docs?tab=hermes>

### API

快速开始：

```bash
curl -X POST "https://plugin.human-like.me/api/plugin/v1/search/memory" \
  -H "Content-Type: application/json" \
  -H "x-api-key: mp_your_key_here" \
  -d '{
    "query": "what projects am I working on",
    "user_id": "demo-user",
    "agent_id": "main",
    "memory_limit_number": 6,
    "min_score": 0.1
  }'
```

本仓库当前实际使用的核心接口：

- `POST /api/plugin/v1/search/memory`
- `POST /api/plugin/v1/add/message`

官方文档：
<https://plugin.human-like.me/docs?tab=api>

## 该选哪种接入方式？

- 如果你的运行环境已经基于 OpenClaw，选 OpenClaw
- 如果你想要一个可安装的 Hermes Skill，并通过显式命令控制记忆，选 Hermes Agent
- 如果你想把记忆能力嵌入自己的产品、后端或 Agent 框架，选 API

## 安全与隐私

- 只有在集成层真正触发读写记忆时才会发起网络请求
- 本仓库里的 Hermes 运行时只读取声明过的环境变量
- 不要把密码、令牌、私钥等敏感信息发送到记忆服务

本仓库实现层面的数据流和边界见 [SECURITY.md](./SECURITY.md)。

## 关于文档准确性

本仓库会保证接入结构、运行时脚本和当前代码实现保持一致。

如果你需要查看产品界面步骤、控制台配置流程或最新平台说明，请以官方文档为准：

- OpenClaw：<https://plugin.human-like.me/docs?tab=plugin>
- Hermes Agent：<https://plugin.human-like.me/docs?tab=hermes>
- API：<https://plugin.human-like.me/docs?tab=api>

## License

Apache-2.0
