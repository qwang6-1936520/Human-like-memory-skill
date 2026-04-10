# Security Notes

This skill is designed for explicit, reviewable, smart-trigger usage.

## Security Posture

- No automatic every-turn recall
- No hook-level silent background save
- No direct reads from local secret files
- No reads from arbitrary local files, shell history, or browser data
- Configuration from runtime environment variables only

## What Triggers Network Requests

Network requests happen only when one of these commands is executed:

- `recall`
- `search`
- `save`
- `save-batch`

If the skill is not invoked, it does not contact the remote service.

## What Data Is Sent

### `recall` / `search`

- `query`
- `user_id`
- `agent_id`
- retrieval tuning fields (`memory_limit_number`, `min_score`)

### `save` / `save-batch`

- explicit message content passed to the command
- `user_id`
- `agent_id`
- generated `conversation_id`
- fixed tag `humanlike-memory-skill`
- session metadata for grouping

## What Is Not Read Or Sent

- local secret files (including OpenClaw/Hermes private files)
- arbitrary local files
- shell history
- unrelated environment variables
- browser data

## Runtime Configuration Contract

The CLI reads only these environment variables:

- `HUMAN_LIKE_MEM_API_KEY`
- `HUMAN_LIKE_MEM_BASE_URL`
- `HUMAN_LIKE_MEM_USER_ID`
- `HUMAN_LIKE_MEM_AGENT_ID`
- optional tuning vars:
  - `HUMAN_LIKE_MEM_LIMIT_NUMBER`
  - `HUMAN_LIKE_MEM_MIN_SCORE`
  - `HUMAN_LIKE_MEM_TIMEOUT_MS`
  - `HUMAN_LIKE_MEM_RECALL_ENABLED`
  - `HUMAN_LIKE_MEM_ADD_ENABLED`
  - `HUMAN_LIKE_MEM_AUTO_SAVE_ENABLED`
  - `HUMAN_LIKE_MEM_SAVE_TRIGGER_TURNS`
  - `HUMAN_LIKE_MEM_SAVE_MAX_MESSAGES`

## Privacy Guidance

- Only send content you are comfortable storing on your configured memory server
- Do not send passwords, private keys, tokens, or other secrets
- Review service privacy policy: <https://plugin.human-like.me/privacy>

## Source

- GitHub: <https://github.com/qwang6-1936520/Human-like-memory-skill>
