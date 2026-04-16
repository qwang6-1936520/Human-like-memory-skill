# OpenClaw Integration

This repository keeps OpenClaw compatibility artifacts so the Human-Like Memory package can still be wired into OpenClaw-based workflows.

Official documentation:

- <https://plugin.human-like.me/docs?tab=plugin>

## Repository Artifacts

- compatibility metadata: [compat/openclaw/skill.json](../../compat/openclaw/skill.json)
- publish metadata: [compat/openclaw/_meta.json](../../compat/openclaw/_meta.json)
- compatibility notes: [compat/openclaw/README.md](../../compat/openclaw/README.md)

## Quick Start

```bash
openclaw plugin install human-like-mem
openclaw config set plugins.entries.human-like-mem.config.apiKey "mp_your_key_here"
openclaw config set plugins.slots.memory human-like-mem
openclaw plugin status human-like-mem
```

## How It Works

- before response: the plugin retrieves relevant memories and injects them into context
- after response: the plugin caches the conversation and flushes memory by turn threshold or timeout
- agent tools: the agent can call `memory_search` and `memory_store`

## Optional Configuration

Defaults called out in the official docs:

- `timeoutMs=15000`
- `turnThreshold=5`
- `autoFlushTimeout=300000`
- `memoryLimitNumber=6`

## What This Path Is For

Choose OpenClaw integration when:

- your runtime is already organized around OpenClaw skills or plugins
- you want to preserve the older OpenClaw packaging shape
- you need an OpenClaw-facing entry point instead of the Hermes skill manifest

## Compatibility Notes

The repo also keeps an environment-variable-driven debug/runtime entry point:

```bash
export HUMAN_LIKE_MEM_API_KEY="mp_your_key_here"
export HUMAN_LIKE_MEM_BASE_URL="https://plugin.human-like.me"
export HUMAN_LIKE_MEM_USER_ID="openclaw-user"
export HUMAN_LIKE_MEM_AGENT_ID="main"
```

The actual source implementation lives in [scripts/memory.mjs](../../scripts/memory.mjs), but for OpenClaw product setup the plugin flow above is the correct entry point.

## Recommended Usage

Use the official OpenClaw documentation for:

- package registration steps
- OpenClaw-specific config wiring
- dashboard or marketplace flows
- any UI-specific installation instructions

This repo keeps the compatibility artifacts and runtime behavior available, but the official docs should remain the source of truth for the OpenClaw product flow.
