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
