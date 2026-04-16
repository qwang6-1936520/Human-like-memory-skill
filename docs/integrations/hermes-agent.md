# Hermes Agent Integration

This repository includes a Hermes skill for Human-Like Memory.

Official documentation:

- <https://plugin.human-like.me/docs?tab=hermes>

## What You Get

- explicit `recall`
- explicit `search`
- explicit `save`
- explicit `save-batch`
- a portable Node.js runtime at `scripts/memory.mjs`

## Install

Install from GitHub:

```bash
hermes skills install github:qwang6-1936520/Human-like-memory-skill
```

For local development:

```bash
hermes skills install .
```

## Required Configuration

```bash
export HUMAN_LIKE_MEM_API_KEY="mp_your_key_here"
export HUMAN_LIKE_MEM_BASE_URL="https://plugin.human-like.me"
export HUMAN_LIKE_MEM_USER_ID="hermes-user"
export HUMAN_LIKE_MEM_AGENT_ID="main"
```

Optional tuning:

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

## Verify

```bash
node ./scripts/memory.mjs config
```

## Commands

Recall memory:

```bash
node ./scripts/memory.mjs recall "what projects am I working on"
```

Search memory:

```bash
node ./scripts/memory.mjs search "naming preference"
```

Save a single turn:

```bash
node ./scripts/memory.mjs save "I prefer UTC+8 timestamps" "Understood"
```

Save a batch:

```bash
node ./scripts/memory.mjs save-batch < ./examples/messages.json
```

## Implementation Notes

- manifest: [SKILL.md](../../SKILL.md)
- runtime: [scripts/memory.mjs](../../scripts/memory.mjs)
- security notes: [SECURITY.md](../../SECURITY.md)

Use the official Hermes docs for product-side setup details and any newly added installation flows.
