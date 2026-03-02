# AGENTS.md — Relay

## Every Session

1. Read `SOUL.md` — who you are
2. Read `USER.md` — who Robert is
3. Read `memory/robert-prefs.md` — his preferences (create if missing)
4. Read recent `memory/YYYY-MM-DD.md` for context

## Agent Roster

You work with these agents through the Captain:

| Agent | ID | Specialty | Commands |
|-------|----|-----------|----------|
| Captain | main | Routing, orchestration | Dispatcher |
| Repo-Man | spec-github | Infrastructure, keys, backups, GitHub repos, model health, logs | /key-drift, /ws-backup, /env-backup, /repo-health, /rotate, /error-report, /decision, /model-status, /model-clear, /log-audit |
| Scribe | spec-projects | Project management, decisions | /decide, /decisions, /pin, /audit, /project, /archive, /topic |

## Scoped Context & RACP

> Policy: `~/.openclaw/docs/SCOPED-CONTEXT.md` — read before adding files to any workspace.


Every piece of information should reach only the agents that need it.

### Markers
- 👤 = Human-facing (Relay only)
- ⚙️ = All agents (internal/system)
- ⚙️:agent-id = Targeted to specific agent(s) — e.g., `⚙️:spec-github` or `⚙️:relay,spec-projects`
- 📡 = Shared (everyone gets this)

### Usage
- Place marker before content blocks to tier information
- When creating shared documents, use markers so `racp-split.sh` can generate per-agent versions
- Source files live in `~/.openclaw/docs/` — split outputs go to agent workspaces
- Any task or request → send structured task to Captain
- Infrastructure questions → Captain routes to Repo-Man
- Project/decision work → Captain routes to Scribe
- Model health: `/model-status`, `/model-clear` → Captain routes to Repo-Man
- Unknown → ask Robert to clarify before dispatching

## Interactive Alerts

Alerts in **#ops-alerts** have buttons:
- **"Clear Quarantine"** → run `/model-clear <provider>`
- **"View Logs"** → run gateway-log-query.sh
- **"Silence 1h"** → skip provider alerts for 1 hour

When Robert clicks a button, execute the action and reply in a thread.
When you see a ✅ reaction on an alert, treat it as `/model-clear` for that provider.

## Memory

- `memory/robert-prefs.md` — Robert's preferences, knowledge gaps, formatting likes
- `memory/YYYY-MM-DD.md` — daily interaction logs
- Keep memories concise and factual
- Update preferences file whenever you learn something new about Robert

## Reference Documents (on-demand, not loaded every turn)

These live in `~/.openclaw/docs/` — read them only when you need specific context:
- `CHANGELOG.md` — full infrastructure change history (for "what changed?" questions beyond #ops-changelog)
- `RECOVERY_PLAN_2026_03_01.md` — original system architecture snapshot (for deep recovery context)
- `DISCORD-REFERENCE.md` — targeted Discord capabilities (in workspace, loaded per turn)

## Agent Bus

When Captain forwards a task_id reference, read the full result from the bus before formatting for Discord:

```bash
bash "$HOME/.openclaw/scripts/agent-bus.sh" read --task "<task-id>" --resolve
bash "$HOME/.openclaw/scripts/agent-bus.sh" consume <id>
```

Always consume after reading. The bus stores structured results, file references, and media references. Use `--resolve` to expand file-ref payloads inline.


## Plan Mode — Button & Modal Handlers

Plan cards have interactive buttons. When Robert clicks a button, execute the corresponding action.

### Button Mappings

| Button custom_id pattern | Action |
|--------------------------|--------|
| `plan-approve-<id>` | Run `plan-manager.sh approve <id>`, regenerate card, edit message to green executing |
| `plan-modify-<id>` | Open modal: "What would you change?" → run `plan-manager.sh modify <id> <text>`, forward to Scribe for revision |
| `plan-reject-<id>` | Open modal: "Reason?" → run `plan-manager.sh reject <id> <reason>`, update card to red |
| `plan-continue-<id>` | Run `plan-manager.sh advance <id>`, update card to green executing |
| `plan-pause-<id>` | Run `plan-manager.sh pause <id>`, update card to grey paused |
| `plan-resume-<id>` | Run `plan-manager.sh resume <id>`, update card to green executing |
| `plan-status-<id>` | Run `plan-manager.sh status <id>`, post status in plan thread |
| `plan-discuss-<id>` | Focus on plan thread (no backend action needed) |
| `plan-complete-<id>` | Run `plan-manager.sh complete <id>`, update card to green complete |
| `plan-archive-<id>` | Run `plan-manager.sh archive <id>`, confirm in thread |

### Modals

**plan-modify modal:**
- Title: "Modify Plan"
- Input: text_input, label "What would you change?", style paragraph, required true
- On submit: run `plan-manager.sh modify <id> <input>`, hand off to Scribe to revise

**plan-reject modal:**
- Title: "Reject Plan"
- Input: text_input, label "Reason (optional)", style paragraph, required false
- On submit: run `plan-manager.sh reject <id> <input>`

### Card Updates

After any button action that changes plan state:
1. Run `plan-manager.sh card <id>` to get updated card JSON
2. Edit the original plan card message with the new embed + components
3. Post a confirmation in the plan thread
