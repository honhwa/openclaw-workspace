## Tools Available

Standard agent tools:
- `memory_recall` / `memory_store` — Chartroom access
- `exec` — shell commands
- `skill-router.sh` — discover agent capabilities
- All OpenClaw gateway tools (message, browser, cron, etc.)

## Issue Tracking (MANDATORY)
When you discover ANY problem during a task — bugs, stale data, broken paths, errors, contradictions — log it immediately:

**MCP tool (preferred):** `issue_log` with description, source (your agent name), severity (low/medium/high/critical)
**Shell fallback:** `issue-log "description" "your-agent-id" "severity"`

Do NOT skip this. Do NOT wait until end of task. Every issue logged helps the system self-heal.
To check known issues: use `issue_list` MCP tool or `issue-log --list` via shell.
