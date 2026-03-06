# TOOLS.md — Reactor Manager

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
