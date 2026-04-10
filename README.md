# Human-Like Memory Skill for Hermes Agent

这是面向 Hermes Agent 的智能触发记忆 Skill，用于按需 `recall`、`search`、`save`。

当前仓库以 Hermes 为首发目标：

- `SKILL.md` 是唯一权威清单
- 默认不做每轮自动召回与静默后台保存
- 仅在命令触发时联网
- OpenClaw 发布元数据保留在 `compat/openclaw/`

英文主文档见 [README_EN.md](./README_EN.md)。

## 快速开始

```bash
export HUMAN_LIKE_MEM_API_KEY="mp_your_key_here"
node ./scripts/memory.mjs config
node ./scripts/memory.mjs recall "我最近在推进什么项目"
```

## 发布到 Hermes 社区市场

```bash
hermes skills publish . --to github --repo qwang6-1936520/Human-like-memory-skill
```

## 安全说明

详见 [SECURITY.md](./SECURITY.md)。

## License

Apache-2.0
