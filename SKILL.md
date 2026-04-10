---
name: human-like-memory
description: "Smart-trigger memory recall, search, and save commands for Human-Like Memory."
version: 1.0.0
author: hanlaomo
license: Apache-2.0
homepage: https://plugin.human-like.me
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [memory, long-term-memory, recall, search, personalization]
required_environment_variables:
  - name: HUMAN_LIKE_MEM_API_KEY
    prompt: "Human-Like Memory API key (mp_xxx)"
    help: "Get a key from https://plugin.human-like.me"
    required_for: "Remote memory read/write API access"
  - name: HUMAN_LIKE_MEM_BASE_URL
    prompt: "Memory API base URL"
    help: "Default is https://plugin.human-like.me"
    required_for: "Custom/self-hosted API endpoint"
  - name: HUMAN_LIKE_MEM_USER_ID
    prompt: "Memory user id"
    help: "Logical user identifier used by memory retrieval"
    required_for: "Cross-session memory identity"
  - name: HUMAN_LIKE_MEM_AGENT_ID
    prompt: "Memory agent id"
    help: "Logical agent identifier used by memory retrieval"
    required_for: "Agent-scoped memory partition"
---

# Human-Like Memory Skill

Agent-usable long-term memory tools for Human-Like Memory.

This skill supports:

- Agent-triggered recall and save when the model judges memory is useful
- Explicit user invocation when the user directly asks to recall or store memory

This skill is intentionally **not always-on**:

- It should **not** recall on every turn by default
- It should **not** silently save every conversation turn by default
- It reads only environment variables exposed to the runtime
- It does **not** read local secret files or per-skill private config files

If you want always-on automatic recall/storage, use the Human-Like Memory plugin instead of this skill.

## Data And Network Disclosure

When the agent or user invokes this skill:

- `recall` / `search` sends your query, `user_id`, and `agent_id` to `https://plugin.human-like.me` or your configured base URL
- `save` / `save-batch` sends the conversation content you explicitly pass to the same service
- No local files, shell history, or unrelated environment variables are read or uploaded

## Use When

- You want to explicitly search past memory
- You want to explicitly save a user fact, decision, preference, or summary
- The agent needs past context to answer well
- The agent should save a durable preference, decision, correction, or summary

## Do Not Use When

- You want every-turn automatic recall or hook-level background saving
- You need hidden or silent network activity
- You want zero remote data transfer

## Setup

### 1. Get API Key

Visit [plugin.human-like.me](https://plugin.human-like.me) and copy your `mp_xxx` key.

### 2. Configure Through Hermes

```bash
export HUMAN_LIKE_MEM_API_KEY="mp_your_key_here"
export HUMAN_LIKE_MEM_BASE_URL="https://plugin.human-like.me"
export HUMAN_LIKE_MEM_USER_ID="hermes-user"
export HUMAN_LIKE_MEM_AGENT_ID="main"
```

You can also use Hermes secure env setup when prompted at skill load time.

### 3. Verify

```bash
node {baseDir}/scripts/memory.mjs config
```

Expected output includes:

```json
{
  "apiKeyConfigured": true
}
```

## Commands

### Recall Or Search Memory

```bash
node {baseDir}/scripts/memory.mjs recall "<query>"
node {baseDir}/scripts/memory.mjs search "<query>"
```

### Save One Turn

```bash
node {baseDir}/scripts/memory.mjs save "<user_message>" "<assistant_response>"
```

### Save A Batch

```bash
echo '[{"role":"user","content":"..."},{"role":"assistant","content":"..."}]' | node {baseDir}/scripts/memory.mjs save-batch
```

### Inspect Runtime Configuration

```bash
node {baseDir}/scripts/memory.mjs config
```

## Agent Invocation Style

This skill may be called by the agent when memory is clearly useful.

Recommended behavior:

- Use `recall` / `search` when the user references prior work, prior preferences, prior decisions, or asks to continue something from earlier
- Use `save` when the user explicitly asks to remember something, corrects identity details, states a stable preference, or confirms an important decision
- Use `save-batch` after a meaningful multi-turn discussion if `HUMAN_LIKE_MEM_AUTO_SAVE_ENABLED=true` and the current conversation has accumulated roughly `HUMAN_LIKE_MEM_SAVE_TRIGGER_TURNS` turns
- Do not call memory APIs for simple greetings or generic single-turn questions with no continuity value

This gives the agent autonomy, but keeps the skill in a smart-trigger mode instead of an always-on mode.

## Suggested Queries

- Recall: `"roadmap decisions from last week"`
- Search: `"what name preference did I mention"`
- Save: `"I prefer UTC+8 timestamps"`

## Save Triggers

The agent should strongly consider `save` when the user:

- states a stable preference
- makes or confirms a decision
- provides a personal profile fact that matters later
- corrects a previous misunderstanding
- explicitly says "remember this"

The agent may use `save-batch` after a longer exchange when a compact summary of the last few turns is likely to help future continuity.

## Error Handling

- If the API key is missing, the script prints exact environment variable setup steps
- If the service times out or errors, nothing is saved implicitly
- If no memories are found, handle that as a normal empty result

## Privacy

- Avoid sending passwords, tokens, or secrets
- Only pass content you are comfortable storing on your configured memory service
- Review [SECURITY.md](./SECURITY.md) for transmitted fields and operational notes

## OpenClaw Compatibility Notes

- OpenClaw metadata artifacts are preserved in `compat/openclaw/`.
- Runtime commands stay compatible (`scripts/memory.mjs` command surface is unchanged).
