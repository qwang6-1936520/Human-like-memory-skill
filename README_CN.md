# Human-Like Memory Skill for Hermes Agent

[English](./README.md) | 中文

Human-Like Memory 是一个给 Hermes Agent 使用的长期记忆 Skill，用于显式地 `recall`、`search` 和 `save`。

它的设计重点是“按需触发”，而不是常驻后台自动记忆：

- 只有在代理或用户真正需要时，才执行 `recall` / `search`
- 只有在需要保存事实、决策或摘要时，才执行 `save` / `save-batch`
- 只有调用这些命令时才会发起网络请求
- 只读取声明过的环境变量，不扫描本地文件

## 它能做什么

- 召回过去的偏好、项目上下文和关键决策
- 当用户提到历史工作或历史对话时检索相关记忆
- 当用户说“记住这个”或确认重要信息时写入长期记忆
- 保持简单稳定的命令面：`recall`、`search`、`save`、`save-batch`、`config`

## 工作原理

这个 Skill 本质上是一个面向 Hermes 的 Human-Like Memory API 封装层。

调用时：

- `recall` 和 `search` 会请求 `/api/plugin/v1/search/memory`
- `save` 和 `save-batch` 会请求 `/api/plugin/v1/add/message`
- API 只接收你传入的 query 或 messages，以及 `user_id`、`agent_id` 和少量调优参数
- 脚本返回结构化 JSON，Hermes 可以直接用于代理上下文

默认服务地址：

```text
https://plugin.human-like.me
```

如果你有自己的服务，可以通过 `HUMAN_LIKE_MEM_BASE_URL` 指向自部署地址。

## 仓库结构

- `SKILL.md`: Hermes 的权威 Skill 清单
- `scripts/memory.mjs`: Node.js CLI 实现
- `examples/messages.json`: `save-batch` 的示例输入
- `compat/openclaw/`: 为兼容保留的 OpenClaw 元数据

## 环境要求

- Node.js 18+
- Hermes Agent CLI
- Human-Like Memory API key

## 安装

从 GitHub 安装：

```bash
hermes skills install github:qwang6-1936520/Human-like-memory-skill
```

本地开发安装：

```bash
hermes skills install .
```

## 配置

先在 <https://plugin.human-like.me> 获取 `mp_xxx` API key，然后设置：

```bash
export HUMAN_LIKE_MEM_API_KEY="mp_your_key_here"
export HUMAN_LIKE_MEM_BASE_URL="https://plugin.human-like.me"
export HUMAN_LIKE_MEM_USER_ID="hermes-user"
export HUMAN_LIKE_MEM_AGENT_ID="main"
```

可选调优参数：

```bash
export HUMAN_LIKE_MEM_LIMIT_NUMBER="6"
export HUMAN_LIKE_MEM_MIN_SCORE="0.1"
export HUMAN_LIKE_MEM_TIMEOUT_MS="30000"
export HUMAN_LIKE_MEM_RECALL_ENABLED="true"
export HUMAN_LIKE_MEM_ADD_ENABLED="true"
export HUMAN_LIKE_MEM_AUTO_SAVE_ENABLED="true"
export HUMAN_LIKE_MEM_SAVE_TRIGGER_TURNS="5"
export HUMAN_LIKE_MEM_SAVE_MAX_MESSAGES="20"
```

验证当前运行配置：

```bash
node ./scripts/memory.mjs config
```

## 用法

召回记忆：

```bash
node ./scripts/memory.mjs recall "我最近在推进什么项目"
```

搜索记忆：

```bash
node ./scripts/memory.mjs search "我的命名偏好"
```

保存单轮对话：

```bash
node ./scripts/memory.mjs save "我更喜欢 UTC+8 时间戳" "收到，我会记住。"
```

批量保存消息：

```bash
node ./scripts/memory.mjs save-batch < ./examples/messages.json
```

## 适用场景

适合：

- 用户提到过去的工作、偏好或决策
- 代理需要跨会话连续性
- 用户明确要求“记住这件事”

不适合：

- 每轮自动召回
- 静默后台保存
- 完全本地、零网络传输的记忆系统

## 自部署与接入

如果你运行自己的 Human-Like Memory 服务，可以这样接入：

```bash
export HUMAN_LIKE_MEM_BASE_URL="https://your-memory-service.example.com"
```

接入建议：

- `HUMAN_LIKE_MEM_USER_ID` 对同一用户保持稳定
- `HUMAN_LIKE_MEM_AGENT_ID` 对同一代理人格或工作区保持稳定
- 根据业务场景调整 `HUMAN_LIKE_MEM_LIMIT_NUMBER`、`HUMAN_LIKE_MEM_MIN_SCORE` 和 `HUMAN_LIKE_MEM_TIMEOUT_MS`
- 如需关闭召回或写入，可显式设置 `HUMAN_LIKE_MEM_RECALL_ENABLED=false` 或 `HUMAN_LIKE_MEM_ADD_ENABLED=false`

## 安全与隐私

- 只有调用记忆命令时才会发送数据
- 不会读取任意本地文件、shell 历史或无关环境变量
- 不要把密钥、密码、私钥等敏感信息发给记忆服务

详细的数据传输与安全边界见 [SECURITY.md](./SECURITY.md)。

## OpenClaw 兼容说明

仓库保留了 `compat/openclaw/` 下的历史元数据，但文档与主入口现在以 Hermes Skill 为主。

## License

Apache-2.0
