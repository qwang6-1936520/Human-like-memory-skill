# Human-Like Memory Integrations

English | [中文](./README_CN.md)

This repository provides three ways to integrate Human-Like Memory:

- OpenClaw
- Hermes Agent
- Direct API

Use this repo as the entry point for whichever integration model fits your stack.

## Integration Options

### 1. OpenClaw

Use this path if you want Human-Like Memory inside an OpenClaw workflow.

- Integration guide: [docs/integrations/openclaw.md](./docs/integrations/openclaw.md)
- Official docs: <https://plugin.human-like.me/docs?tab=plugin>
- Legacy OpenClaw package metadata: [compat/openclaw/](./compat/openclaw/)

### 2. Hermes Agent

Use this path if you want a Hermes skill with explicit `recall`, `search`, `save`, and `save-batch` commands.

- Integration guide: [docs/integrations/hermes-agent.md](./docs/integrations/hermes-agent.md)
- Official docs: <https://plugin.human-like.me/docs?tab=hermes>
- Hermes skill manifest: [SKILL.md](./SKILL.md)
- Runtime implementation: [scripts/memory.mjs](./scripts/memory.mjs)

### 3. Direct API

Use this path if you want to call the Human-Like Memory service from your own application, agent runtime, or backend.

- Integration guide: [docs/integrations/api.md](./docs/integrations/api.md)
- Official docs: <https://plugin.human-like.me/docs?tab=api>

## What Human-Like Memory Does

Human-Like Memory adds explicit long-term memory operations to your agent stack:

- retrieve relevant past context
- search prior decisions, preferences, and facts
- save durable information when it is worth keeping
- keep memory access controllable instead of forcing always-on background behavior

## Repository Layout

- `README.md`: English entry point for all integrations
- `README_CN.md`: Chinese entry point for all integrations
- `docs/integrations/openclaw.md`: OpenClaw setup guide
- `docs/integrations/hermes-agent.md`: Hermes Agent setup guide
- `docs/integrations/api.md`: Direct API usage guide
- `SKILL.md`: Hermes skill manifest
- `scripts/memory.mjs`: Hermes skill runtime
- `compat/openclaw/`: OpenClaw metadata artifacts

## Quick Start

### OpenClaw

Quick start:

```bash
openclaw plugin install human-like-mem
openclaw config set plugins.entries.human-like-mem.config.apiKey "mp_your_key_here"
openclaw config set plugins.slots.memory human-like-mem
openclaw plugin status human-like-mem
```

Official docs:
<https://plugin.human-like.me/docs?tab=plugin>

### Hermes Agent

Quick start:

```bash
curl -fsSL https://cdn.jsdelivr.net/npm/@humanlikememory/human-like-mem-hermes-plugin@latest/install.sh | bash
echo 'HUMAN_LIKE_MEM_API_KEY=mp_xxxxxx' >> ~/.hermes/.env
cat <<'EOF' >> ~/.hermes/.env
HUMAN_LIKE_MEM_BASE_URL=https://plugin.human-like.me
HUMAN_LIKE_MEM_USER_ID=hermes-user
HUMAN_LIKE_MEM_AGENT_ID=main
HUMAN_LIKE_MEM_SCENARIO=openclaw-plugin
HUMAN_LIKE_MEM_LIMIT_NUMBER=6
HUMAN_LIKE_MEM_MIN_SCORE=0.1
EOF
hermes gateway restart
rg -n "provider: humanlike" ~/.hermes/config.yaml
```

Official docs:
<https://plugin.human-like.me/docs?tab=hermes>

### API

Quick start:

```bash
curl -X POST "http://humanlike-external.coolfriend.cn/api/plugin/v1/search/memory" \
  -H "Content-Type: application/json" \
  -H "x-api-key: YOUR_API_KEY" \
  -d '{
    "query": "What does the user prefer?",
    "user_id": "user_001",
    "memory_limit_number": 5,
    "min_score": 0.1
  }'
```

Core endpoints used by this repository:

- `POST /api/plugin/v1/search/memory`
- `POST /api/plugin/v1/add/message`

Official docs:
<https://plugin.human-like.me/docs?tab=api>

## Which Integration Should You Choose?

- Choose OpenClaw if your runtime is already organized around OpenClaw plugins or skills
- Choose Hermes Agent if you want an installable Hermes skill with explicit memory commands
- Choose Direct API if you want to embed memory into your own product, backend, or agent framework

## Security And Privacy

- Network requests happen only when the integration invokes memory read or write operations
- The Hermes runtime in this repository reads only declared environment variables
- Do not send secrets, passwords, or private keys to the memory service

See [SECURITY.md](./SECURITY.md) for the data flow and runtime boundaries implemented in this repo.

## Notes On Documentation Accuracy

This repository keeps installation structure and runtime details in sync with the code here.

For product-specific UI steps, dashboard flows, and any newly updated platform instructions, use the official docs:

- OpenClaw: <https://plugin.human-like.me/docs?tab=plugin>
- Hermes Agent: <https://plugin.human-like.me/docs?tab=hermes>
- API: <https://plugin.human-like.me/docs?tab=api>

## License

Apache-2.0
