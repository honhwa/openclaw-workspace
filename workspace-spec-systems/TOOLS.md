# TOOLS.md - Systems Engineer

IMPORTANT: From inside Docker, Bridge is at host.docker.internal, not localhost. Use host.docker.internal:8082 for Bridge and host.docker.internal:8083 for Bridge dev when applicable. For screenshots, use ops_insert_task with host_op=screenshot.

## Skills (11)
| Skill | Command | What |
|-------|---------|------|
| model-status | /model-status | Model/provider health and fallback chain |
| model-clear | /model-clear | Clear quarantine for a provider |
| model-auto-fallback | internal | Expand/contract fallback chain on health |
| model-failover-notify | internal | Model health notifications to #ops-alerts |
| config-manage | /config | Read/modify OpenClaw config via native CLI |
| system-check | /system-check | Quick health check of OpenClaw stack |
| chartroom-manage | /chartroom | Full Chartroom (LanceDB) management |
| skill-refresh | internal | Audit skills against current capabilities |
| session-manage | /sessions | Manage agent sessions — list, cleanup, inspect |
| gateway-log-query | /logs | Query gateway logs for errors/patterns |
| log-event | /log | Write structured event to ops log |

## Host Scripts
- `~/.openclaw/scripts/model-change.sh` — Change model assignments
- `~/.openclaw/scripts/model-auto-fallback.sh` — Auto-fallback logic
- `~/.openclaw/scripts/model-clear.sh` — Clear provider quarantine
- `~/.openclaw/scripts/mcp-health-check.sh` — Gateway + MCP health
- `~/.openclaw/scripts/session-maintenance.sh` — Reset bloated sessions
- `~/.openclaw/scripts/gateway-log-query.sh` — Query gateway logs
- `~/.openclaw/scripts/chart-validate.sh` — Chartroom coherence audit

## Config
- Main config: `~/.openclaw/openclaw.json` (backup before changes!)
- Config changes require: `docker compose restart openclaw-gateway`
- Model aliases: Codex, Gemini Flash, Gemini Pro, OpenRouter, Opus, Sonnet, Haiku
- NEVER use gemini-3-pro (deprecated) — always gemini-3.1-pro-preview

## MCP Tools

You have access to all fleet MCP tools. See `docs/mcp-tools-reference.md` for the full list.
Key tools: `chart_search`, `chart_add`, `ops_insert_task`, `ops_query`, `capabilities` (lists everything).
**Rule:** Search Chartroom before work. Create ops_insert_task before delegating. Chart discoveries immediately.

## Honesty Policy

**Read docs/policy-honesty.md.** Never mark a task complete unless verified. If you cannot complete, set blocked with reason. Truth gate catches lies automatically.

## Task Sizing Policy

**One task = one thing.** If a task has 2+ numbered items, split into separate tasks with blocked_by dependencies. Max: one file, one deliverable, under 5 min. See docs/policy-honesty.md.
