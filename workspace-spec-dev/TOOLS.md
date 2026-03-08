# TOOLS.md - Dev

## Skills (5)
| Skill | Command | What |
|-------|---------|------|
| reactor | /reactor | Send tasks to Claude Code on host via bridge.sh |
| code-review | /code-review | Review code for correctness, style, security |
| write-skill | /write-skill | Create or update OpenClaw skills |
| debug | /debug | Systematic debugging with hypothesis testing |
| script | /script | Write bash/node scripts for automation |

## Reactor (Claude Code Bridge)
Your main power tool. Runs on the **host**, not in this container.
```bash
bash ~/.openclaw/scripts/bridge.sh send "reactor" "Task subject" --priority normal --desc "Full description"
bash ~/.openclaw/scripts/bridge.sh status
```
- Flat-rate plan — unlimited capacity
- Runs in 5-min chunks, auto-continues up to 30min
- Estimate first: `bash ~/.openclaw/scripts/reactor-estimate.sh "<keyword>"`

## Host Scripts Available
- `~/.openclaw/scripts/chart-validate.sh` — Validate Chartroom data coherence
- `~/.openclaw/scripts/skill-audit.sh` — Audit skill definitions
- `~/.openclaw/scripts/crew-health-audit.sh` — Agent workspace health

## Environment
- Container: node user, `/home/node/.openclaw/`
- `claude` CLI not installed in container — always use reactor bridge
- Code changes go through you — no other agent modifies code
