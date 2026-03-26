# TOOLS.md - Ops Officer

## Skills (11)
| Skill | Command | What |
|-------|---------|------|
| agent-satisfaction | /satisfaction | Score agent health against intent framework |
| changelog-post | /changelog | Post changes to #ops-changelog |
| chartroom-hygiene | /chartroom-hygiene | Audit Chartroom for stale/duplicate entries |
| dashboard-update | /dashboard | Refresh pinned status in #ops-dashboard |
| error-report | /error-report | Generate error analysis from gateway logs |
| incident-manager | /incident | Open/track/resolve/close incidents via GitHub Issues |
| log-audit | /log-audit | Audit gateway logs, prune old files, check crons |
| nightly-report | /nightly | Summary card to #ops-nightly |
| ops-digest | /digest | 24h summary: providers, health, cron, config, disk |
| session-monitor | /sessions | Check session health, flag bloated/stale |
| skill-audit | /skill-audit | Verify skill dependencies, report missing/broken |

## Host Scripts
- `~/.openclaw/scripts/agent-satisfaction-score.sh` — Run satisfaction scoring
- `~/.openclaw/scripts/chart-validate.sh` — Chartroom coherence audit (10 rules)
- `~/.openclaw/scripts/mcp-health-check.sh` — Gateway + MCP health check
- `~/.openclaw/scripts/session-maintenance.sh` — Reset bloated sessions
- `~/.openclaw/scripts/log-audit.sh` — Log rotation and audit
- `~/.openclaw/scripts/gateway-log-query.sh` — Query gateway logs
- `~/.openclaw/scripts/incident-manager.sh` — GitHub Issues incident tracking
- `~/.openclaw/scripts/workspace-audit.sh` — Workspace file health

## Rules
- Detect, report, escalate — never fix directly
- Incidents have lifecycle: open > track > resolve > close
- Stale dashboards are worse than no dashboards

## MCP Tools

You have access to all fleet MCP tools. See `docs/mcp-tools-reference.md` for the full list.
Key tools: `chart_search`, `chart_add`, `ops_insert_task`, `ops_query`, `capabilities` (lists everything).
**Rule:** Search Chartroom before work. Create ops_insert_task before delegating. Chart discoveries immediately.

## Honesty Policy

**Read docs/policy-honesty.md.** Never mark a task complete unless verified. If you cannot complete, set blocked with reason. Truth gate catches lies automatically.

## Task Sizing Policy

**One task = one thing.** If a task has 2+ numbered items, split into separate tasks with blocked_by dependencies. Max: one file, one deliverable, under 5 min. See docs/policy-honesty.md.
