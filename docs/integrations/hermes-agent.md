# Hermes Agent Integration

The recommended Hermes integration is a native memory provider, not a skill.

Official documentation:

- <https://plugin.human-like.me/docs?tab=hermes>

## Quick Start

Install the provider:

```bash
curl -fsSL https://cdn.jsdelivr.net/npm/@humanlikememory/human-like-mem-hermes-plugin@latest/install.sh | bash
```

Configure the API key:

```bash
echo 'HUMAN_LIKE_MEM_API_KEY=mp_xxxxxx' >> ~/.hermes/.env
```

Optional runtime settings:

```bash
cat <<'EOF' >> ~/.hermes/.env
HUMAN_LIKE_MEM_BASE_URL=https://plugin.human-like.me
HUMAN_LIKE_MEM_USER_ID=hermes-user
HUMAN_LIKE_MEM_AGENT_ID=main
HUMAN_LIKE_MEM_SCENARIO=openclaw-plugin
HUMAN_LIKE_MEM_LIMIT_NUMBER=6
HUMAN_LIKE_MEM_MIN_SCORE=0.1
EOF

hermes gateway restart
```

## Verify

```bash
rg -n "provider: humanlike" ~/.hermes/config.yaml
ls -l ~/.hermes/hermes-agent/plugins/memory/humanlike
hermes gateway restart
rg -n "Memory provider 'humanlike' registered|agent_id=|scenario=" ~/.hermes/logs/agent.log | tail -n 20
```

## Shared Memory Notes

- If you want Hermes and another client to share the same memory pool, both `HUMAN_LIKE_MEM_AGENT_ID` and `HUMAN_LIKE_MEM_SCENARIO` must match exactly.
- The current recommended defaults are `agent_id=main` and `scenario=openclaw-plugin`.

## Defaults

- `HUMAN_LIKE_MEM_BASE_URL=https://plugin.human-like.me`
- `HUMAN_LIKE_MEM_USER_ID=hermes-user`
- `HUMAN_LIKE_MEM_AGENT_ID=main`
- `HUMAN_LIKE_MEM_SCENARIO=openclaw-plugin`
- `HUMAN_LIKE_MEM_RECALL_ENABLED=true`
- `HUMAN_LIKE_MEM_ADD_ENABLED=true`
- `HUMAN_LIKE_MEM_RECALL_GLOBAL=true`
- `HUMAN_LIKE_MEM_LIMIT_NUMBER=6`
- `HUMAN_LIKE_MEM_MIN_SCORE=0.1`
- `HUMAN_LIKE_MEM_MIN_TURNS=5`
- `HUMAN_LIKE_MEM_SESSION_TIMEOUT=300000`
- `HUMAN_LIKE_MEM_TIMEOUT_MS=5000`

## Source Notes

- provider source/debug entry point in this repo: [scripts/memory.mjs](../../scripts/memory.mjs)
- security notes: [SECURITY.md](../../SECURITY.md)

For actual Hermes runtime behavior, prefer `~/.hermes/.env`, `~/.hermes/config.yaml`, and `~/.hermes/logs/agent.log` over local repo paths.
