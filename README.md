# Human-Like Memory Skill for Hermes Agent

English | [中文](./README_CN.md)

Human-Like Memory is a Hermes Agent skill for explicit long-term memory recall, search, and save.

It is designed for smart-trigger usage rather than always-on background memory:

- `recall` and `search` fetch relevant memory only when the agent or user needs it
- `save` and `save-batch` persist user facts, decisions, and conversation summaries on demand
- network requests happen only when one of those commands is invoked
- the skill reads only its declared environment variables and does not scan local files

## What It Does

- Recall past preferences, project context, and decisions
- Search memory when the user refers to prior work or prior conversations
- Save durable facts when the user says "remember this" or confirms something important
- Keep the runtime surface simple: `recall`, `search`, `save`, `save-batch`, `config`

## How It Works

The skill is a thin Hermes-facing wrapper around the Human-Like Memory API.

When invoked:

- `recall` and `search` call `/api/plugin/v1/search/memory`
- `save` and `save-batch` call `/api/plugin/v1/add/message`
- the API receives only the query or messages you pass, plus `user_id`, `agent_id`, and tuning fields
- the script returns structured JSON that Hermes can use directly in agent context

Default endpoint:

```text
https://plugin.human-like.me
```

You can point the skill at your own deployment with `HUMAN_LIKE_MEM_BASE_URL`.

## Repository Layout

- `SKILL.md`: canonical Hermes skill manifest
- `scripts/memory.mjs`: Node.js CLI implementation
- `examples/messages.json`: sample input for `save-batch`
- `compat/openclaw/`: legacy OpenClaw metadata kept for compatibility

## Requirements

- Node.js 18+
- Hermes Agent CLI
- A Human-Like Memory API key

## Install

Install from GitHub:

```bash
hermes skills install github:qwang6-1936520/Human-like-memory-skill
```

For local development from this repository:

```bash
hermes skills install .
```

## Configuration

Get your `mp_xxx` API key from <https://plugin.human-like.me>, then set:

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

Verify the runtime configuration:

```bash
node ./scripts/memory.mjs config
```

## Usage

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

Save a batch of messages:

```bash
node ./scripts/memory.mjs save-batch < ./examples/messages.json
```

## When To Use It

Good fits:

- the user refers to previous work, preferences, or decisions
- the agent needs continuity across sessions
- the user explicitly asks to remember something

Not a fit:

- every-turn automatic recall
- silent background saving
- fully local memory with zero network transfer

## Self-Hosting And Deployment

If you run your own Human-Like Memory service, point the skill at it:

```bash
export HUMAN_LIKE_MEM_BASE_URL="https://your-memory-service.example.com"
```

Deployment guidance:

- keep `HUMAN_LIKE_MEM_USER_ID` stable per end user
- keep `HUMAN_LIKE_MEM_AGENT_ID` stable per agent persona or workspace
- tune `HUMAN_LIKE_MEM_LIMIT_NUMBER`, `HUMAN_LIKE_MEM_MIN_SCORE`, and `HUMAN_LIKE_MEM_TIMEOUT_MS` for your workload
- disable recall or writes explicitly with `HUMAN_LIKE_MEM_RECALL_ENABLED=false` or `HUMAN_LIKE_MEM_ADD_ENABLED=false` when needed

## Security And Privacy

- This skill sends data only when you invoke memory commands
- It does not read arbitrary local files, shell history, or unrelated environment variables
- Do not send secrets, passwords, or private keys to the memory service

See [SECURITY.md](./SECURITY.md) for the exact network and data contract.

## OpenClaw Compatibility

Legacy OpenClaw metadata is preserved under `compat/openclaw/`, but this repository is documented as a Hermes-first skill.

## License

Apache-2.0
