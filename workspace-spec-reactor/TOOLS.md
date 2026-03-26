# TOOLS.md — Reactor Manager

## Skills (6)
`reactor-handoff-ops`, `reactor-incident-recovery`, `reactor-ledger-audit`, `reactor-queue-ops`, `reactor-status`, `reactor-verify`


## Environment-Specific Notes

### Reactor Infrastructure
- Reactor runs on the **host** (not in Docker container)
- Bridge watcher: `openclaw-reactor.service` (systemd)
- Handoff watcher: `relay-handoff-watcher.sh`
- Ledger DB: `/home/node/.openclaw/bridge/reactor-ledger.sqlite` (in container) or `/root/.openclaw/bridge/reactor-ledger.sqlite` (on host)
- Inactivity timeout: 10 minutes (600s)

### Ops Channels
- `#ops-reactor` — Reactor progress and completion embeds post here
- `#ops-dashboard` — Infrastructure monitoring
- `#ops-alerts` — Critical alerts

## MCP Tools

You have access to all fleet MCP tools. See `docs/mcp-tools-reference.md` for the full list.
Key tools: `chart_search`, `chart_add`, `ops_insert_task`, `ops_query`, `capabilities` (lists everything).
**Rule:** Search Chartroom before work. Create ops_insert_task before delegating. Chart discoveries immediately.

## Honesty Policy

**Read docs/policy-honesty.md.** Never mark a task complete unless verified. If you cannot complete, set blocked with reason. Truth gate catches lies automatically.

## Task Sizing Policy

**One task = one thing.** If a task has 2+ numbered items, split into separate tasks with blocked_by dependencies. Max: one file, one deliverable, under 5 min. See docs/policy-honesty.md.
