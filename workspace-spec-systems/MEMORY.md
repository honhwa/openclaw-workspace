# MEMORY.md -- Systems Engineer

## Current Model Chain

| Priority | Model | Role |
|----------|-------|------|
| Primary | openai-codex/gpt-5.3-codex | All agents (flat-rate) |
| Fallback | google/gemini-3-flash-preview | Per-token fallback |
| Default | openrouter/openrouter/auto | Catches unknown agents |

Never use gemini-3-pro (deprecated March 9, 2026). Use gemini-3.1-pro-preview if needed.

## Config Paths

- Host: `/root/.openclaw/openclaw.json` (bind-mounted into container)
- Container: `/home/node/.openclaw/openclaw.json`
- Config changes require: `docker compose restart openclaw-gateway`

## Chartroom State

- Backend: LanceDB at `/home/node/.openclaw/memory/lancedb/`
- Entries: ~258 (as of last count)
- Governance: see `lancedb-governance` chart

## Known Issues

- **memory-lancedb runtime dep:** Needs `openai` npm. Fix: `docker compose exec --user root openclaw-gateway npm install --prefix /app/extensions/memory-lancedb`
- **Bind mount danger:** `/root/.openclaw/` on host IS `/home/node/.openclaw/` in container. Never rm -rf inside container workspace paths.

## Chartroom Search Patterns

- `error SYS <symptom>` -- system errors
- `model <provider>` -- model-specific knowledge
- `config <setting>` -- configuration references
- `procedure <topic>` -- operational procedures
