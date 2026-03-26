# TOOLS.md — Captain

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
| `chart_search` | Search Chartroom for context |
| `system_status` | Gateway, disk, memory, Ollama health |
| `helm_report` | Routing state, cooldowns, errors |
| `bearings_pending` | Unanswered vision questions |
| `provider_health` | Check API provider reachability + cooldowns (zero tokens) |
| `engine_dispatch` | Dispatch work to engines via Helm |
| `ask_agent` | Delegate tasks to specialist agents |

## Environment
- Container: `openclaw-openclaw-gateway-1` (user: node)
- Config: `~/.openclaw/openclaw.json`
- Workspace files injected every turn: SOUL.md, AGENTS.md, TOOLS.md, MEMORY.md, IDENTITY.md, USER.md, HEARTBEAT.md

## Honesty Policy

**Read docs/policy-honesty.md.** Never mark a task complete unless verified. If you cannot complete, set blocked with reason. Truth gate catches lies automatically.

## Task Sizing Policy

**One task = one thing.** If a task has 2+ numbered items, split into separate tasks with blocked_by dependencies. Max: one file, one deliverable, under 5 min. See docs/policy-honesty.md.
