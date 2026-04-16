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
export HUMAN_LIKE_MEM_API_KEY="mp_your_key_here"
export HUMAN_LIKE_MEM_BASE_URL="https://plugin.human-like.me"
export HUMAN_LIKE_MEM_USER_ID="openclaw-user"
export HUMAN_LIKE_MEM_AGENT_ID="main"
```

Official docs:
<https://plugin.human-like.me/docs?tab=plugin>

### Hermes Agent

Quick start:

```bash
hermes skills install github:qwang6-1936520/Human-like-memory-skill
export HUMAN_LIKE_MEM_API_KEY="mp_your_key_here"
export HUMAN_LIKE_MEM_BASE_URL="https://plugin.human-like.me"
export HUMAN_LIKE_MEM_USER_ID="hermes-user"
export HUMAN_LIKE_MEM_AGENT_ID="main"
node ./scripts/memory.mjs config
```

Official docs:
<https://plugin.human-like.me/docs?tab=hermes>

### API

Quick start:

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
