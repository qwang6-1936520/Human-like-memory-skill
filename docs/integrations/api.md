# Direct API Integration

Use this path when you want to integrate Human-Like Memory directly into your own application, service, or agent runtime without going through OpenClaw or Hermes Agent packaging.

Official documentation:

- <https://plugin.human-like.me/docs?tab=api>

## Base URL

```text
https://plugin.human-like.me
```

## Endpoints Used In This Repository

This repository currently calls:

- `POST /api/plugin/v1/search/memory`
- `POST /api/plugin/v1/add/message`

## Authentication

Requests are authenticated with:

```text
x-api-key: mp_xxx
```

The Hermes runtime in this repo also sends:

- `x-request-id`
- `x-plugin-version`
- `x-client-type`

## Search Example

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

## Save Example

```bash
curl -X POST "https://plugin.human-like.me/api/plugin/v1/add/message" \
  -H "Content-Type: application/json" \
  -H "x-api-key: mp_your_key_here" \
  -d '{
    "user_id": "demo-user",
    "conversation_id": "session-123",
    "agent_id": "main",
    "messages": [
      { "role": "user", "content": "I prefer UTC+8 timestamps" },
      { "role": "assistant", "content": "Understood." }
    ],
    "tags": ["humanlike-memory-skill"],
    "async_mode": true
  }'
```

## Data Model Notes

Based on the runtime implementation in this repository:

- search requests send `query`, `user_id`, `agent_id`, `memory_limit_number`, and `min_score`
- save requests send `user_id`, `conversation_id`, `agent_id`, and message arrays
- the runtime also attaches metadata for session grouping

## Source References

- implementation: [scripts/memory.mjs](../../scripts/memory.mjs)
- security notes: [SECURITY.md](../../SECURITY.md)

Use the official API docs for the latest request and response schemas, newly added fields, and product-side examples.
