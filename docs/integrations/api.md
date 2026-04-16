# Direct API Integration

Use this path when you want to integrate Human-Like Memory directly into your own application, service, or agent runtime without going through OpenClaw or Hermes Agent packaging.

Official documentation:

- <https://plugin.human-like.me/docs?tab=api>

## Base URL

```text
http://humanlike-external.coolfriend.cn
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

## Save Example

```bash
curl -X POST "http://humanlike-external.coolfriend.cn/api/plugin/v1/add/message" \
  -H "Content-Type: application/json" \
  -H "x-api-key: YOUR_API_KEY" \
  -d '{
    "conversation_id": "conv_001",
    "user_id": "user_001",
    "messages": [
      { "role": "user", "content": "I prefer dark mode for all interfaces" },
      { "role": "assistant", "content": "Got it, I will remember your dark mode preference!" }
    ],
    "tags": ["preference"],
    "async_mode": true
  }'
```

## Data Model Notes

Based on the official API docs and the runtime implementation in this repository:

- search requests send `query`, optional `user_id`, optional `tags`, optional `source`, `memory_limit_number`, and `min_score`
- save requests send `messages`, optional `user_id`, optional `conversation_id`, optional `tags`, and `async_mode`
- the runtime also attaches metadata for session grouping

## Source References

- implementation: [scripts/memory.mjs](../../scripts/memory.mjs)
- security notes: [SECURITY.md](../../SECURITY.md)

Use the official API docs for the latest request and response schemas, newly added fields, and product-side examples.
