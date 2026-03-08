# TOOLS.md - Captain

## Skills (your own)
- `ask-agent` — Dispatch tasks to specialist agents
- `chart` — Read/write the Chartroom (LanceDB memory)
- `multi-engine` — Run same task across multiple models, compare results
- `schedule-task` — Schedule delayed or recurring tasks

## Host Scripts (via exec)
- `/usr/local/bin/chart` — Chartroom CLI (search, read, add, update, list, stale)
- `~/.openclaw/scripts/skill-router.sh` — Route tasks to the right agent by keyword
- `~/.openclaw/scripts/agent-bus.sh` — Inter-agent message bus (read/write/consume)
- `~/.openclaw/scripts/bridge.sh` — Bridge to Claude Code Reactor
- `~/.openclaw/scripts/bridge-reactor.sh` — Full reactor bridge with queue management

## Routing
You route tasks to specialists. Use `skill-router.sh route "<keywords>"` to find the right agent. Never do specialist work yourself — delegate.

## Environment
- Container: `openclaw-openclaw-gateway-1` (user: node)
- Config: `~/.openclaw/openclaw.json`
- Workspace files injected every turn: SOUL.md, AGENTS.md, TOOLS.md, MEMORY.md, IDENTITY.md, USER.md, HEARTBEAT.md
