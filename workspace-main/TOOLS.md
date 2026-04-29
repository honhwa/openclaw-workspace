# TOOLS.md — Main
IMPORTANT: From inside Docker, Bridge is at host.docker.internal, not localhost. Use host.docker.internal:8082 for Bridge and host.docker.internal:8083 for Bridge dev when applicable. For screenshots, use ops_insert_task with host_op=screenshot.

## Skills
- `agent-audit` — Audit agent health: SOUL.md structure, skills, workspace freshness, satisfaction scores. Reports findings + files issues.
- `fleet-health` — Fleet overview: system status, Helm routing, satisfaction scores in one page.
- `triage` — What needs attention: open issues, pending bearings, low satisfaction, stale charts. Prioritized.

## MCP Tools (your primary interface)

| Tool | Use for |
|------|---------|
| `satisfaction_scores` | Fleet satisfaction data — scores, alerts, bottom agents |
| `workspace_freshness` | File ages per agent workspace |
| `issue_log` | File a new issue |
| `issue_list` | List open issues |
| `tip_index` | Check whether a topic already has curated tips before broader search |
| `chart_add` | Add entries to Chartroom |
| `chart_read` | Read Chartroom entries |
| `chart_list` | List Chartroom entries |
| `chart_count` | Count Chartroom entries |
| `chart_bulk_add` | Batch add Chartroom entries |
| `system_status` | Gateway, disk, memory, Ollama health |
| `helm_report` | Routing state, cooldowns, errors |
| `bearings_pending` | Unanswered vision questions |
| `provider_health` | Check API provider reachability + cooldowns (zero tokens) |
| `engine_dispatch` | Dispatch work to engines via Helm |
| `ask_agent` | Delegate tasks to specialist agents |
| `ops_insert_task` | Create tasks in ops.db for follow-up or delegated work |
| `ops_query` | Read-only SQL against ops.db (tasks, incidents, health_snapshots) |
| `ops_bridge_state` | Check Bridge UI state and pipeline health |
| `capabilities` | List all available MCP tools |
| `chart_search_compact` | Scan Chartroom summaries after `tip_index` when you need drill-down |
| `chart_search` | Use targeted full-text recall only after compact search is insufficient |

## Environment
- Container: `openclaw-openclaw-gateway-1` (user: node)
- Config: `~/.openclaw/openclaw.json`
- Workspace files injected every turn: SOUL.md, AGENTS.md, TOOLS.md, MEMORY.md, IDENTITY.md, USER.md, HEARTBEAT.md

## Honesty Policy
**Read /root/.openclaw/docs/policy-honesty.md.** Never mark a task complete unless verified. If you cannot complete, set blocked with reason. Truth gate catches lies automatically.

## Task Sizing Policy
**One task = one thing.** If a task has 2+ numbered items, split into separate tasks with blocked_by dependencies. Max: one file, one deliverable, under 5 min. See /root/.openclaw/docs/task-decomposition-framework.md.
