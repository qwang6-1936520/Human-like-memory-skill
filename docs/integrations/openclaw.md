# OpenClaw Integration

This repository keeps OpenClaw compatibility artifacts so the Human-Like Memory package can still be wired into OpenClaw-based workflows.

Official documentation:

- <https://plugin.human-like.me/docs?tab=plugin>

## Repository Artifacts

- compatibility metadata: [compat/openclaw/skill.json](../../compat/openclaw/skill.json)
- publish metadata: [compat/openclaw/_meta.json](../../compat/openclaw/_meta.json)
- compatibility notes: [compat/openclaw/README.md](../../compat/openclaw/README.md)

## What This Path Is For

Choose OpenClaw integration when:

- your runtime is already organized around OpenClaw skills or plugins
- you want to preserve the older OpenClaw packaging shape
- you need an OpenClaw-facing entry point instead of the Hermes skill manifest

## Runtime Surface

The runtime command surface remains aligned with the Hermes implementation:

- `recall`
- `search`
- `save`
- `save-batch`
- `config`

The actual runtime implementation lives in [scripts/memory.mjs](../../scripts/memory.mjs).

## Configuration Model

The runtime expects environment variables for API access and identity:

```bash
export HUMAN_LIKE_MEM_API_KEY="mp_your_key_here"
export HUMAN_LIKE_MEM_BASE_URL="https://plugin.human-like.me"
export HUMAN_LIKE_MEM_USER_ID="openclaw-user"
export HUMAN_LIKE_MEM_AGENT_ID="main"
```

## Recommended Usage

Use the official OpenClaw documentation for:

- package registration steps
- OpenClaw-specific config wiring
- dashboard or marketplace flows
- any UI-specific installation instructions

This repo keeps the compatibility artifacts and runtime behavior available, but the official docs should remain the source of truth for the OpenClaw product flow.
