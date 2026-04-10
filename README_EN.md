# Human-Like Memory Skill for Hermes Agent

Smart-trigger long-term memory skill for Hermes Agent.

This skill is intentionally scoped for explicit and context-driven usage:

- recall or save only when memory is useful
- no every-turn automatic recall
- no hook-level silent background save
- no direct reads from local secret files
- configuration via Hermes env passthrough or explicit environment injection

[Chinese README](README.md)

## Network Behavior

Network requests happen only when the skill is invoked:

- `recall` / `search` sends query + `user_id` + `agent_id`
- `save` / `save-batch` sends the message content passed into the command

Default endpoint: `https://plugin.human-like.me`

## Repository Structure

- `SKILL.md` - canonical Hermes skill manifest
- `scripts/memory.mjs` - portable Node CLI implementation
- `compat/openclaw/` - legacy OpenClaw publish metadata (`skill.json`, `_meta.json`)

## Prerequisites

- Node.js 18+
- Hermes Agent CLI

## Install

After publish, install from GitHub-backed hub:

```bash
hermes skills install github:qwang6-1936520/humanlike-memory-skill
```

For local testing:

```bash
hermes skills install .
```

## Configuration

### 1. Get API Key

Get your `mp_xxx` key from <https://plugin.human-like.me>.

### 2. Set Environment Variables

```bash
export HUMAN_LIKE_MEM_API_KEY="mp_your_key_here"
export HUMAN_LIKE_MEM_BASE_URL="https://plugin.human-like.me"
export HUMAN_LIKE_MEM_USER_ID="hermes-user"
export HUMAN_LIKE_MEM_AGENT_ID="main"
export HUMAN_LIKE_MEM_RECALL_ENABLED="true"
export HUMAN_LIKE_MEM_ADD_ENABLED="true"
export HUMAN_LIKE_MEM_AUTO_SAVE_ENABLED="true"
export HUMAN_LIKE_MEM_SAVE_TRIGGER_TURNS="5"
```

### 3. Verify

```bash
node ./scripts/memory.mjs config
```

Expected output includes:

```json
{
  "apiKeyConfigured": true
}
```

## CLI Usage

```bash
node ./scripts/memory.mjs recall "what projects am I working on"
node ./scripts/memory.mjs search "naming preference"
node ./scripts/memory.mjs save "I prefer UTC+8 timestamps" "Understood"
echo '[{"role":"user","content":"Hi"},{"role":"assistant","content":"Hello"}]' | node ./scripts/memory.mjs save-batch
```

## Publish to Hermes Community Hub

```bash
hermes skills publish . --to github --repo qwang6-1936520/humanlike-memory-skill
```

## Official Optional-Skills Readiness

This repository is prepared for community publishing first. For official optional-skills consideration:

- keep security/disclosure docs up to date
- keep metadata Hermes-native and scanner-friendly
- submit through the official Hermes contribution/review path

## OpenClaw Compatibility

Runtime command surface remains compatible. Legacy OpenClaw metadata is preserved under `compat/openclaw/`.

## Security

See [SECURITY.md](./SECURITY.md) for transmitted fields and operational notes.

## License

Apache-2.0
